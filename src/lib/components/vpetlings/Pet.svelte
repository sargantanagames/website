<script lang="ts">
  import { onMount } from 'svelte';

  import idleGif from '$lib/assets/vpet-idle.gif';
  import walkGif from '$lib/assets/vpet-walk.gif';

  const SPEED = 0.3; // px per ms
  const RADIUS = 128;

  // duration of ONE idle animation loop (ms)
  const IDLE_DURATION = 1000;
  const IDLE_LOOPS = 3;

  let container: HTMLDivElement;
  let raf: number;

  // center-based position
  let x = 0;
  let y = 0;
  let targetX = 0;
  let targetY = 0;

  let state: 'idle' | 'walk' = 'idle';
  let idleElapsed = 0;

  let mouseX = 0;
  let mouseY = 0;
  let lastTime = 0;

  // direction the sprite is facing
  let facing: 'left' | 'right' = 'right';

  function handleMouseMove(e: MouseEvent) {
    mouseX = e.clientX;
    mouseY = e.clientY;
  }

  function pickTarget() {
    const rect = container.getBoundingClientRect();

    const originX = mouseX || rect.left + rect.width / 2;
    const originY = mouseY || rect.top + rect.height / 2;

    const angle = Math.random() * Math.PI * 2;
    const r = Math.random() * RADIUS;

    targetX = originX + Math.cos(angle) * r - rect.left;
    targetY = originY + Math.sin(angle) * r - rect.top;

    console.log(`New target: (${targetX.toFixed(2)}, ${targetY.toFixed(2)})`);

    state = 'walk';
  }

  function loop(time: number) {
    const dt = time - lastTime;
    lastTime = time;

    if (state === 'idle') {
      idleElapsed += dt;

      if (idleElapsed >= IDLE_DURATION * IDLE_LOOPS) {
        idleElapsed = 0;
        pickTarget();
      }
    }

    if (state === 'walk') {
      const dx = targetX - x;
      const dy = targetY - y;
      const dist = Math.hypot(dx, dy);

      // update facing direction
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
    const rect = container.getBoundingClientRect();

    // exact center start
    x = rect.width / 2;
    y = rect.height / 2;

    window.addEventListener('mousemove', handleMouseMove);
    raf = requestAnimationFrame(loop);

    return () => {
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
    class="pointer-events-none absolute select-none h-48 w-48"
    style="
      transform:
        translate({x}px, {y}px)
        translate(-50%, -50%)
        scaleX({facing === 'left' ? -1 : 1});
    "
  />
</div>
