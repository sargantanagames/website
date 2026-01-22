<script lang="ts">
  import { onMount } from 'svelte';

  import idleGif from '$lib/assets/vpet-idle.gif';
  import walkGif from '$lib/assets/vpet-walk.gif';

  const SPEED = 0.3; // px per ms
  const IDLE_DURATION = 1000;
  const IDLE_LOOPS = 3;
  const MOUSE_IDLE_RADIUS = 120; // px

  let container: HTMLDivElement;
  let raf: number;

  let x = 118;
  let y = 118;
  let targetX = 0;
  let targetY = 0;

  let state: 'idle' | 'walk' = 'idle';
  let idleElapsed = 0;

  let mouseX = 0;
  let mouseY = 0;
  let lastTime = 0;

  let facing: 'left' | 'right' = 'right';

  let radius = 0;

  function reset() {
    const rect = container.getBoundingClientRect();

    x = rect.width / 2;
    y = rect.height / 2;

    targetX = x;
    targetY = y;

    state = 'idle';
    idleElapsed = 0;
    lastTime = performance.now();

    radius = Math.min(rect.width, rect.height);
  }

  function handleMouseMove(e: MouseEvent) {
    mouseX = e.clientX;
    mouseY = e.clientY;
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
    const dt = time - lastTime;
    lastTime = time;

    if (state === 'idle') {
      if (isMouseNear()) {
        idleElapsed = 0; // freeze idle indefinitely
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
    reset();

    window.addEventListener('mousemove', handleMouseMove);
    window.addEventListener('resize', reset);

    raf = requestAnimationFrame(loop);

    return () => {
      window.removeEventListener('mousemove', handleMouseMove);
      window.removeEventListener('resize', reset);
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
    class="pointer-events-none absolute select-none h-48 w-48"
    style="
      transform:
        translate({x}px, {y}px)
        translate(-50%, -50%)
        scaleX({facing === 'left' ? -1 : 1});
    "
  />
</div>
