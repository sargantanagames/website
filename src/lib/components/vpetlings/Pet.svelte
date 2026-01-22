<script lang="ts">
  import { onMount } from 'svelte';

  import idleGif from '$lib/assets/vpet-idle.gif';
  import walkGif from '$lib/assets/vpet-walk.gif';

  // ⬇️ MUST be nullable
  export let featureImage: HTMLImageElement | null = null;

  const BASE_SPEED = 0.0003;
  const IDLE_DURATION = 1000;
  const IDLE_LOOPS = 2;
  const MOUSE_IDLE_RADIUS = 120;

  const SPAWN_X = 0.09;
  const SPAWN_Y = 0.48;

  let container: HTMLDivElement | null = null;
  let raf: number;

  let x = -10000;
  let y = -10000;
  let targetX = 0;
  let targetY = 0;

  let state: 'idle' | 'walk' = 'idle';
  let idleElapsed = 0;

  let mouseX = 0;
  let mouseY = 0;
  let lastTime = 0;

  let facing: 'left' | 'right' = 'left';

  let isActive = false;
  let petSize = 144;

  function handleMouseMove(e: MouseEvent) {
    mouseX = e.clientX;
    mouseY = e.clientY;
  }

  function spawnFromImage(nx: number, ny: number) {
    if (!featureImage || !container) return;

    const imageRect = featureImage.getBoundingClientRect();
    const containerRect = container.getBoundingClientRect();

    x = imageRect.left + imageRect.width * nx - containerRect.left;
    y = imageRect.top + imageRect.height * ny - containerRect.top;

    targetX = x;
    targetY = y;

    petSize = imageRect.width * 0.17;
  }

  function isMouseNear() {
    if (!container) return false;

    const rect = container.getBoundingClientRect();
    const petScreenX = rect.left + x;
    const petScreenY = rect.top + y;

    return Math.hypot(mouseX - petScreenX, mouseY - petScreenY) < MOUSE_IDLE_RADIUS;
  }

  function pickTarget() {
    if (!container) return;

    const rect = container.getBoundingClientRect();

    if (rect.width === 0 || rect.height === 0) {
      state = 'idle';
      return;
    }

    const minX = petSize / 2;
    const minY = petSize / 2;
    const maxX = rect.width - petSize / 2;
    const maxY = rect.height - petSize / 2;

    const originX = mouseX || rect.left + rect.width / 2;
    const originY = mouseY || rect.top + rect.height / 2;

    const radius = MOUSE_IDLE_RADIUS;
    const angle = Math.random() * Math.PI * 2;
    const distance = Math.random() * radius;

    let tx = originX + Math.cos(angle) * distance - rect.left;
    let ty = originY + Math.sin(angle) * distance - rect.top;

    tx = Math.min(maxX, Math.max(minX, tx));
    ty = Math.min(maxY, Math.max(minY, ty));

    targetX = tx;
    targetY = ty;

    state = 'walk';
  }

  function loop(time: number) {
    raf = requestAnimationFrame(loop);

    if (!isActive || !container) return;

    const dt = time - lastTime;
    lastTime = time;

    if (state === 'idle') {
      if (isMouseNear()) {
        idleElapsed = 0;
      } else {
        idleElapsed += dt;
        if (idleElapsed >= IDLE_DURATION * IDLE_LOOPS) {
          idleElapsed = 0;
          pickTarget();
        }
      }
    }

    if (state === 'walk') {
      const dx = targetX - x;
      const dy = targetY - y;
      const dist = Math.hypot(dx, dy);

      if (Math.abs(dx) > 1) {
        facing = dx < 0 ? 'left' : 'right';
      }

      if (dist < 10) {
        x = targetX;
        y = targetY;
        state = 'idle';
        idleElapsed = 0;
      } else {
        const rect = container.getBoundingClientRect();
        const viewportScale = Math.min(rect.width, rect.height);
        const speed = BASE_SPEED * viewportScale;

        x += (dx / dist) * speed * dt;
        y += (dy / dist) * speed * dt;
      }
    }
  }

  function updateContainerHeight() {
    if (!container || !isActive) return;

    container.style.height = '0px';
    container.style.height = `${document.body.scrollHeight}px`;
  }

  function handleResize() {
    spawnFromImage(SPAWN_X, SPAWN_Y);
    updateContainerHeight();
  }

  function reparentToWorld() {
    if (!container) return;

    const world = container.parentElement?.parentElement;
    if (!world) return;

    world.appendChild(container);
  }

  onMount(() => {
    const waitForAnchor = () => {
      if (container && featureImage) {
        spawnFromImage(SPAWN_X, SPAWN_Y);
        updateContainerHeight();

        const revealEl = featureImage.closest('.reveal') as HTMLElement | null;

        const observer = new IntersectionObserver(
          ([entry]) => {
            if (entry.isIntersecting && !isActive && revealEl) {
              const onDone = (e: TransitionEvent) => {
                if (e.target !== revealEl) return;

                revealEl.removeEventListener('transitionend', onDone);

                isActive = true;
                lastTime = performance.now();

                reparentToWorld();
                spawnFromImage(SPAWN_X, SPAWN_Y);
              };

              revealEl.addEventListener('transitionend', onDone, { once: true });
            }
          },
          { threshold: 1 }
        );

        observer.observe(featureImage);

        window.addEventListener('pointermove', handleMouseMove);
        window.addEventListener('resize', handleResize);

        raf = requestAnimationFrame(loop);

        return () => {
          observer.disconnect();
          window.removeEventListener('pointermove', handleMouseMove);
          window.removeEventListener('resize', handleResize);
          cancelAnimationFrame(raf);
        };
      }

      requestAnimationFrame(waitForAnchor);
    };

    waitForAnchor();
  });

</script>

<div
  bind:this={container}
  class="absolute top-0 left-0 w-full pointer-events-none z-50"
>
  <img
    src={state === 'idle' ? idleGif : walkGif}
    alt="Virtual pet"
    class="pointer-events-none absolute select-none"
    style="
      width: {petSize}px;
      height: {petSize}px;
      transform:
        translate({x}px, {y}px)
        translate(-50%, -50%)
        scaleX({facing === 'left' ? -1 : 1});
    "
  />
</div>
