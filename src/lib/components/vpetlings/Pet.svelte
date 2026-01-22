<script lang="ts">
  import { onMount } from 'svelte';

  import idleGif from '$lib/assets/vpet-idle.gif';
  import walkGif from '$lib/assets/vpet-walk.gif';

  export let featureImage: HTMLImageElement;

  const SPEED = 0.3;
  const IDLE_DURATION = 1000;
  const IDLE_LOOPS = 33;
  const MOUSE_IDLE_RADIUS = 120;

  let container: HTMLDivElement;
  let raf: number;

  let x = 0;
  let y = 0;
  let targetX = 0;
  let targetY = 0;

  let state: 'idle' | 'walk' = 'idle';
  let idleElapsed = 0;

  let mouseX = 0;
  let mouseY = 0;
  let lastTime = 0;

  let facing: 'left' | 'right' = 'left';
  let radius = 0;

  let isActive = false;

  function handleMouseMove(e: MouseEvent) {
    mouseX = e.clientX;
    mouseY = e.clientY;
  }

  function spawnFromImage(px: number, py: number) {
    const imageRect = featureImage.getBoundingClientRect();
    const containerRect = container.getBoundingClientRect();

    x = imageRect.left + px - containerRect.left;
    y = imageRect.top + py - containerRect.top - 20;

    targetX = x;
    targetY = y;
  }

  function isMouseNear() {
    const rect = container.getBoundingClientRect();
    const petScreenX = rect.left + x;
    const petScreenY = rect.top + y;

    return Math.hypot(mouseX - petScreenX, mouseY - petScreenY) < MOUSE_IDLE_RADIUS;
  }

  function pickTarget() {
    const rect = container.getBoundingClientRect();

    const originX = mouseX || rect.left + rect.width / 2;
    const originY = mouseY || rect.top + rect.height / 2;

    const angle = Math.random() * Math.PI * 2;

    targetX = originX + Math.cos(angle) * radius - rect.left;
    targetY = originY + Math.sin(angle) * radius - rect.top;

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

  onMount(() => {
    const observer = new IntersectionObserver(
      ([entry]) => {
        if (entry.isIntersecting && !isActive) {
          isActive = true;
          spawnFromImage(96, 114);
          lastTime = performance.now();
        }
      },
      { threshold: 0.3 }
    );

    observer.observe(featureImage);

    window.addEventListener('mousemove', handleMouseMove);

    const rect = container.getBoundingClientRect();
    radius = Math.min(rect.width, rect.height);

    raf = requestAnimationFrame(loop);

    return () => {
      observer.disconnect();
      window.removeEventListener('mousemove', handleMouseMove);
      cancelAnimationFrame(raf);
    };
  });
</script>

<div
  bind:this={container}
  class="relative h-full w-full z-50"
>
  <img
    src={state === 'idle' ? idleGif : walkGif}
    alt="Virtual pet"
    class="pointer-events-none absolute select-none h-36 w-36"
    style="
      transform:
        translate({x}px, {y}px)
        translate(-50%, -50%)
        scaleX({facing === 'left' ? -1 : 1});
    "
  />
</div>
