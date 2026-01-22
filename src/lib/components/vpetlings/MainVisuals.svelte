<script lang="ts">
  import { onMount, onDestroy } from 'svelte';

  import img0 from '$lib/assets/vpetlings/screenshots/img.png';
  import img1 from '$lib/assets/vpetlings/screenshots/img_1.png';
  import img2 from '$lib/assets/vpetlings/screenshots/img_2.png';
  import img3 from '$lib/assets/vpetlings/screenshots/img_3.png';
  import img4 from '$lib/assets/vpetlings/screenshots/img_4.png';
  import img5 from '$lib/assets/vpetlings/screenshots/img_5.png';
  import img6 from '$lib/assets/vpetlings/screenshots/img_6.png';
  import img7 from '$lib/assets/vpetlings/screenshots/img_7.png';

  const images = [img0, img1, img2, img3, img4, img5, img6, img7];

  let index = 0;
  let interval: number;

  function next() {
    index = (index + 1) % images.length;
  }

  function select(i: number) {
    index = i;
    resetInterval();
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

<section class="w-full p-16">
  <img
    src={images[index]}
    alt="VPetlings gameplay screenshot"
    class="mx-auto h-full w-full border-2 border-text rounded bg-bg object-contain"
  />

  <div class="mt-4 flex justify-center gap-2 w-full">
    {#each images as _, i}
      <button
        aria-label={`Select image ${i + 1}`}
        onclick={() => select(i)}
        class="
          w-3 h-3 md:w-4 md:h-4
          cursor-pointer
          border-2 border-text
          transition ease-out
          hover:-rotate-45
          {i === index ? 'bg-secondary !rotate-45' : 'bg-accent-dark'}
        "
      />
    {/each}
  </div>
</section>
