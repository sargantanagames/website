<script lang="ts">
  import imageFeature1 from "$lib/assets/vpetlings/features/features-1.webp";
  import imageFeature2 from "$lib/assets/vpetlings/features/features-2.apng";
  import imageFeature3 from "$lib/assets/vpetlings/features/features-3.webp";
  import imageFeature4 from "$lib/assets/vpetlings/features/features-4.apng";
  import imageFeature5 from "$lib/assets/vpetlings/features/features-5.webp";
  import imageFeature6 from "$lib/assets/vpetlings/features/features-6.apng";
  import imageFeature7 from "$lib/assets/vpetlings/features/features-7.webp";
  import imageFeature8 from "$lib/assets/vpetlings/features/features-8.avif";

  import Pet from './Pet.svelte';
  let featureImage: HTMLImageElement;
</script>

<section class="space-y-4 md:space-y-8 text-base md:text-xl text-center">
  <img
    src={imageFeature1}
    alt="Lots of little behaviors"
    class="max-w-full md:max-w-2/3 mx-auto object-contain"
  />

  <p>
    They wander, nap, chase and interact with your cursor, play fetch, get moody,
    and sometimes just vibe. If you need to focus on other tasks, you can send
    them to <span class="text-accent-darker">rest in their little room</span>.
  </p>

  <img
    bind:this={featureImage}
    src={imageFeature2}
    alt="Pet animations"
    class="max-w-full rounded object-contain"
  />


  <img
    src={imageFeature3}
    alt="Make it yours"
    class="max-w-full md:max-w-2/3 mx-auto object-contain"
  />

  <p>
    Decorate their room with furniture and unlock
    <span class="text-accent-darker">unique color variants</span> to fit your
    desktop style.
  </p>

  <img
    src={imageFeature4}
    alt="Rooms"
    class="max-w-full rounded object-contain"
  />

  <p>
    Each pet has different personalities, stat bonuses, and follows a
    <span class="text-accent-darker">real-time sleep schedule</span>.
  </p>

  <img
    src={imageFeature5}
    alt="Discover new VPets"
    class="max-w-full md:max-w-2/3 mx-auto object-contain"
  />

  <p>
    Your pet will evolve as they gain experience over time. When their time’s up,
    they can be reborn into a
    <span class="text-accent-darker">new egg</span>, giving your next companion a
    head start.
  </p>

  <img
    src={imageFeature6}
    alt="Eggs and stage animations"
    class="max-w-full rounded object-contain"
  />

  <p>
    Complete the glossary and discover
    <span class="text-accent-darker">new creatures</span> that await.
  </p>

  <img
    src={imageFeature7}
    alt="Train and battle"
    class="max-w-full md:max-w-2/3 mx-auto object-contain"
  />

  <p>
    Train your pet for battle, explore small worlds, and take part in
    <span class="text-accent-darker">casual, timing-based battles</span> to earn
    coins and unlock new items.
  </p>

  <img
    src={imageFeature8}
    alt="Combat menus"
    class="max-w-full rounded object-contain"
  />

</section>
