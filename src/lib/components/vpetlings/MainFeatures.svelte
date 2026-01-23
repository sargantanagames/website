<script lang="ts">
  import imageLotsOfLittleBehaviours from "$lib/assets/vpetlings/features/lots-of-little-behaviours.webp";
  import imagePetStates from "$lib/assets/vpetlings/features/pet-states.gif";
  import imageMakeItYours from "$lib/assets/vpetlings/features/make-it-yours.webp";
  import imageRooms from "$lib/assets/vpetlings/features/rooms.gif";
  import imageDisoverNewVPets from "$lib/assets/vpetlings/features/discover-new-vpets.webp";
  import imageEggEvolutions from "$lib/assets/vpetlings/features/egg-evolutions.gif";
  import imageTrainAndBattle from "$lib/assets/vpetlings/features/train-and-battle.webp";
  import imageBattle from "$lib/assets/vpetlings/features/battle.webp";

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

<section class="space-y-8 md:space-y-16 text-base md:text-2xl text-center">
  <div class="space-y-2 md:space-y-4">
    <img
      use:inView
      src={imageLotsOfLittleBehaviours}
      alt="Lots of little behaviors"
      class="reveal from-right from-left mx-auto object-contain is-visible pixelated h-6 md:h-8"
    />

    <p>
      They wander, nap, chase and interact with your cursor, play fetch, get moody,
      and sometimes just vibe. If you need to focus on other tasks, you can send
      them to <span class="text-accent-darker">rest in their little room</span>.
    </p>

    <div
      use:inView
      class="reveal from-right w-full inline-block z-50"
    >
      <img
        bind:this={featureImage}
        src={imagePetStates}
        alt="Pet animations"
        class="block max-w-full w-full mx-auto object-contain"
      />

      <Pet featureImage={featureImage} />
    </div>

  </div>

  <div class="space-y-2 md:space-y-4">
    <img
      use:inView
      src={imageMakeItYours}
      alt="Make it yours"
      class="reveal from-left mx-auto object-contain pixelated h-6 md:h-8"
    />

    <p>
      Decorate their room with furniture and unlock
      <span class="text-accent-darker">unique color variants</span> to fit your
      desktop style.
    </p>

    <img
      use:inView
      src={imageRooms}
      alt="Rooms"
      class="reveal from-right max-w-full w-full mx-auto object-contain"
    />

    <p>
      Each pet has different personalities, stat bonuses, and follows a
      <span class="text-accent-darker">real-time sleep schedule</span>.
    </p>
  </div>

  <div class="space-y-2 md:space-y-4">
    <img
      use:inView
      src={imageDisoverNewVPets}
      alt="Discover new VPets"
      class="reveal from-left mx-auto object-contain pixelated h-6 md:h-8"
    />

    <p>
      Your pet will evolve as they gain experience over time. When their time’s up,
      they can be reborn into a
      <span class="text-accent-darker">new egg</span>, giving your next companion a
      head start.
    </p>

    <img
      use:inView
      src={imageEggEvolutions}
      alt="Eggs and stage animations"
      class="reveal from-right max-w-full w-full mx-auto object-contain"
    />

    <p>
      Complete the glossary and discover
      <span class="text-accent-darker">new creatures</span> that await.
    </p>

  </div>

  <div class="space-y-2 md:space-y-4">
    <img
      use:inView
      src={imageTrainAndBattle}
      alt="Train and battle"
      class="reveal from-left mx-auto object-contain pixelated h-6 md:h-8"
    />

    <p>
      Train your pet for battle, explore small worlds, and take part in
      <span class="text-accent-darker">casual, timing-based battles</span> to earn
      coins and unlock new items.
    </p>

    <img
      use:inView
      src={imageBattle}
      alt="Combat menus"
      class="reveal from-right max-w-full w-full mx-auto object-contain"
    />
  </div>

</section>

<style>
    .reveal {
        opacity: 0;
        transition: opacity 1000ms ease,
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
