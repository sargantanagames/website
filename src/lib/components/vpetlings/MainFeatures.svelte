<script lang="ts">
  import imageFeature1 from "$lib/assets/vpetlings/features/features-1.webp";
  import imageFeature2 from "$lib/assets/vpetlings/features/features-2.gif";
  import imageFeature3 from "$lib/assets/vpetlings/features/features-3.webp";
  import imageFeature4 from "$lib/assets/vpetlings/features/features-4.apng";
  import imageFeature5 from "$lib/assets/vpetlings/features/features-5.webp";
  import imageFeature6 from "$lib/assets/vpetlings/features/features-6.apng";
  import imageFeature7 from "$lib/assets/vpetlings/features/features-7.webp";
  import imageFeature8 from "$lib/assets/vpetlings/features/features-8.avif";

  import Pet from "./Pet.svelte";

  let featureImage: HTMLImageElement | null = null;

  function inView(node: HTMLElement) {
    const observer = new IntersectionObserver(
      ([entry]) => {
        if (entry.isIntersecting) {
          node.classList.add("is-visible");
          observer.unobserve(node);
        }
      },
      { threshold: 0.25 }
    );

    observer.observe(node);

    return {
      destroy() {
        observer.disconnect();
      }
    };
  }
</script>

<section class="space-y-4 md:space-y-8 text-base md:text-xl text-center">
  <img
    use:inView
    src={imageFeature1}
    alt="Lots of little behaviors"
    class="reveal from-right from-left max-w-full md:max-w-2/3 mx-auto object-contain is-visible"
  />

  <p>
    They wander, nap, chase and interact with your cursor, play fetch, get moody,
    and sometimes just vibe. If you need to focus on other tasks, you can send
    them to <span class="text-accent-darker">rest in their little room</span>.
  </p>

  <div
    use:inView
    class="reveal from-right relative inline-block z-50"
  >
    <img
      bind:this={featureImage}
      src={imageFeature2}
      alt="Pet animations"
      class="block max-w-full rounded object-contain"
    />

    <Pet featureImage={featureImage} />
  </div>

  <img
    use:inView
    src={imageFeature3}
    alt="Make it yours"
    class="reveal from-left max-w-full md:max-w-2/3 mx-auto object-contain"
  />

  <p>
    Decorate their room with furniture and unlock
    <span class="text-accent-darker">unique color variants</span> to fit your
    desktop style.
  </p>

  <img
    use:inView
    src={imageFeature4}
    alt="Rooms"
    class="reveal from-right max-w-full rounded object-contain"
  />

  <p>
    Each pet has different personalities, stat bonuses, and follows a
    <span class="text-accent-darker">real-time sleep schedule</span>.
  </p>

  <img
    use:inView
    src={imageFeature5}
    alt="Discover new VPets"
    class="reveal from-left max-w-full md:max-w-2/3 mx-auto object-contain"
  />

  <p>
    Your pet will evolve as they gain experience over time. When their time’s up,
    they can be reborn into a
    <span class="text-accent-darker">new egg</span>, giving your next companion a
    head start.
  </p>

  <img
    use:inView
    src={imageFeature6}
    alt="Eggs and stage animations"
    class="reveal from-right max-w-full rounded object-contain"
  />

  <p>
    Complete the glossary and discover
    <span class="text-accent-darker">new creatures</span> that await.
  </p>

  <img
    use:inView
    src={imageFeature7}
    alt="Train and battle"
    class="reveal from-left max-w-full md:max-w-2/3 mx-auto object-contain"
  />

  <p>
    Train your pet for battle, explore small worlds, and take part in
    <span class="text-accent-darker">casual, timing-based battles</span> to earn
    coins and unlock new items.
  </p>

  <img
    use:inView
    src={imageFeature8}
    alt="Combat menus"
    class="reveal from-right max-w-full rounded object-contain"
  />
</section>

<style>
    .reveal {
        opacity: 0;
        transition:
                opacity 800ms ease,
                transform 800ms ease;
        will-change: opacity, transform;
    }

    .reveal.from-left {
        transform: translateX(-128px);
    }

    .reveal.from-right {
        transform: translateX(128px);
    }

    .reveal.is-visible {
        opacity: 1;
        transform: translateX(0);
    }


</style>
