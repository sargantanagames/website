<script lang="ts">
  import { onMount } from 'svelte';

  import idleGif from '$lib/assets/vpet-idle.gif';
  import walkGif from '$lib/assets/vpet-walk.gif';

  export let featureImage: HTMLImageElement;

  const SPEED = 0.3;
  const IDLE_DURATION = 2000;
  const IDLE_LOOPS = 2;
  const MOUSE_IDLE_RADIUS = 120;

  const SPAWN_X = 0.09;
  const SPAWN_Y = 0.48;

  let container: HTMLDivElement;
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
    const imageRect = featureImage.getBoundingClientRect();
    const containerRect = container.getBoundingClientRect();

    x = imageRect.left + imageRect.width * nx - containerRect.left;
    y = imageRect.top + imageRect.height * ny - containerRect.top;

    targetX = x;
    targetY = y;

    petSize = imageRect.width * 0.17;
  }

  function isMouseNear() {
    const rect = container.getBoundingClientRect();
    const petScreenX = rect.left + x;
    const petScreenY = rect.top + y;

    return Math.hypot(mouseX - petScreenX, mouseY - petScreenY) < MOUSE_IDLE_RADIUS;
  }

  function pickTarget() {
    const rect = container.getBoundingClientRect();

    if (rect.width === 0 || rect.height === 0) {
      state = 'idle';
      return;
    }

    const minX = petSize / 2;
    const minY = petSize / 2;
    const maxX = rect.width - petSize / 2;
    const maxY = rect.height - petSize / 2;

    // Fallback: center of screen if mouse hasn't moved yet
    const originX = mouseX || rect.left + rect.width / 2;
    const originY = mouseY || rect.top + rect.height / 2;

    // Pick a random point within a radius around the mouse
    const radius = MOUSE_IDLE_RADIUS;
    const angle = Math.random() * Math.PI * 2;
    const distance = Math.random() * radius;

    let tx = originX + Math.cos(angle) * distance - rect.left;
    let ty = originY + Math.sin(angle) * distance - rect.top;

    // HARD CLAMP to window bounds
    tx = Math.min(maxX, Math.max(minX, tx));
    ty = Math.min(maxY, Math.max(minY, ty));

    targetX = tx;
    targetY = ty;

    state = 'walk';
  }


  function loop(time: number) {
    if (!isActive) {
      raf = requestAnimationFrame(loop);
      return;
    }

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
        x += (dx / dist) * SPEED * dt;
        y += (dy / dist) * SPEED * dt;
      }
    }

    raf = requestAnimationFrame(loop);
  }

  function handleResize() {
    spawnFromImage(SPAWN_X, SPAWN_Y);
    const rect = container.getBoundingClientRect();
  }

  onMount(() => {
    spawnFromImage(SPAWN_X, SPAWN_Y);

    const observer = new IntersectionObserver(
      ([entry]) => {
        if (entry.isIntersecting && !isActive) {
          isActive = true;
          lastTime = performance.now();
        }
      },
      { threshold: 1 }
    );

    observer.observe(featureImage);
    updateContainerHeight();
    window.addEventListener('mousemove', handleMouseMove);
    window.addEventListener('resize', handleResize);

    const rect = container.getBoundingClientRect();

    raf = requestAnimationFrame(loop);

    return () => {
      observer.disconnect();
      window.removeEventListener('mousemove', handleMouseMove);
      window.removeEventListener('resize', handleResize);
      cancelAnimationFrame(raf);
    };
  });

  function updateContainerHeight() {
    container.style.height = `${document.body.scrollHeight}px`;
  }

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
