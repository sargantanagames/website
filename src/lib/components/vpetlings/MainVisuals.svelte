<script lang="ts">
  import { onMount, onDestroy } from 'svelte';

  import screenshot1 from '$lib/assets/vpetlings/screenshots/screenshot_1.webp';
  import screenshot2 from '$lib/assets/vpetlings/screenshots/screenshot_2.webp';
  import screenshot3 from '$lib/assets/vpetlings/screenshots/screenshot_3.webp';
  import screenshot4 from '$lib/assets/vpetlings/screenshots/screenshot_4.webp';
  import screenshot5 from '$lib/assets/vpetlings/screenshots/screenshot_5.webp';
  import screenshot6 from '$lib/assets/vpetlings/screenshots/screenshot_6.webp';
  import screenshot7 from '$lib/assets/vpetlings/screenshots/screenshot_7.webp';
  import screenshot8 from '$lib/assets/vpetlings/screenshots/screenshot_8.webp';

  const images = [
    screenshot1,
    screenshot2,
    screenshot3,
    screenshot4,
    screenshot5,
    screenshot6,
    screenshot7,
    screenshot8
  ];

  let index = 0;
  let interval: number;

  function next() {
    index = (index + 1) % images.length;
  }

  function prev() {
    index = (index - 1 + images.length) % images.length;
  }

  function resetInterval() {
    clearInterval(interval);
    interval = window.setInterval(next, 3000);
  }

  onMount(() => {
    interval = window.setInterval(next, 3000);
  });

  onDestroy(() => {
    clearInterval(interval);
  });
</script>

{#snippet arrowZone({ side, label, onClick, symbol })}
  <div
    aria-label={label}
    onclick={() => {
			onClick();
			resetInterval();
		}}
    class="
			group absolute top-0 h-full w-16 cursor-pointer
			{side === 'left' ? 'left-0' : 'right-0'}
		"
  >
		<span
      class="
				absolute top-1/2 -translate-y-1/2
				text-4xl font-bold text-text
				opacity-60 transition
				group-hover:opacity-100
				group-active:opacity-100
				{side === 'left' ? 'left-4' : 'right-4'}
			"
    >
			{symbol}
		</span>
  </div>
{/snippet}

<section class="w-full">
  <div class="relative mx-auto w-full md:w-5/6">
    <img
      src={images[index]}
      alt="VPetlings gameplay screenshot"
      class="w-full rounded border-2 border-text object-contain"
    />

    {@render arrowZone({
      side: 'left',
      label: 'Previous image',
      onClick: prev,
      symbol: '<'
    })}

    {@render arrowZone({
      side: 'right',
      label: 'Next image',
      onClick: next,
      symbol: '>'
    })}
  </div>

  <div class="mt-4 flex w-full justify-center gap-2">
    {#each images as _, i}
      <button
        aria-label={`Select image ${i + 1}`}
        onclick={() => {
					index = i;
					resetInterval();
				}}
        class="
					h-3 w-3 cursor-pointer border-2 border-text
					transition ease-out hover:-rotate-45
					md:h-4 md:w-4
					{i === index ? '!rotate-45 bg-secondary' : 'bg-accent-dark'}
				"
      />
    {/each}
  </div>
</section>
