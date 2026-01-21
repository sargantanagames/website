<script lang="ts">
  import eggAnimation from '$lib/assets/egg-animation.gif';
  import pinkRoom from '$lib/assets/pink-room.gif';
  import lavaArea from '$lib/assets/lava-area.png';
</script>

<section class="m-4 w-full max-w-96 space-y-16 md:mx-auto md:max-w-2xl">

  <!-- Feature 1 -->
  <div class="flex flex-col items-center gap-4 md:flex-row md:h-28">
    <img
      src={eggAnimation}
      alt="Egg wobbling animation"
      class="h-full max-h-28 object-contain md:h-full md:w-auto"
    />

    <div class="rounded  py-1 px-2 md:text-left text-center">
      <h3 class="mb-2 text-3xl">Choose your own</h3>
      <p class="text-xl">
        Hatch your egg and meet a tiny companion that
        <span class="text-accent-darker"> lives on your desktop</span>,
        always nearby as you go about your day.
      </p>
    </div>
  </div>

  <!-- Feature 2 -->
  <div class="flex flex-col items-center gap-4 md:flex-row-reverse md:h-28">
    <img
      src={pinkRoom}
      alt="Pink themed pet room"
      class="h-full max-h-28 object-contain md:w-auto"
    />

    <div class="rounded  py-1 px-2 md:text-left text-center">
      <h3 class="mb-2 text-3xl">Customize their room</h3>
      <p class="text-xl">
        Decorate your pet’s room with
        <span class="text-accent-darker"> furniture you can purchase and unlock</span>,
        shaping a space that fits your desktop and your companion’s personality.
      </p>
    </div>
  </div>

  <!-- Feature 3 -->
  <div class="flex flex-col items-center gap-4 md:flex-row md:h-28">
    <img
      src={lavaArea}
      alt="Lava themed adventure area"
      class="h-full max-h-16 rounded object-contain md:w-auto"
    />

    <div class="rounded py-1 px-2 md:text-left text-center">
      <h3 class="mb-2 text-3xl">Explore new zones</h3>
      <p class="text-xl">
        Send your pet on
        <span class="text-accent-darker"> small adventures</span>,
        explore new areas, and take part in casual,
        <span class="text-accent-darker"> timing-based battles</span>.
      </p>
    </div>
  </div>

</section>
