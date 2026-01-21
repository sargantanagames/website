<script lang="ts">
	import eggAnimation from '$lib/assets/egg-animation.gif';
	import pinkRoom from '$lib/assets/pink-room.gif';
	import lavaArea from '$lib/assets/lava-area.png';
</script>

<section class="max-w-96 space-y-12 md:max-w-3xl m-4">
	<div class="rounded border-2 border-text bg-accent p-4">
		<div class="flex flex-col items-center gap-2 md:flex-row">
			<img
				src={eggAnimation}
				alt="Egg wobbing animation"
				class="h-24 w-24 rounded border-2 border-text bg-bg object-cover"
			/>

			<div class="text-center md:text-left">
				<h3 class="mb-2 text-2xl">Choose your own</h3>
        <p class="text-lg">
          Hatch your egg and meet a tiny companion that
          <span class="text-accent-darker">lives on your desktop</span>,
          always nearby as you go about your day.
        </p>
			</div>
		</div>
	</div>

	<div class="rounded border-2 border-text bg-accent p-4">
		<div class="flex flex-col items-center gap-2 md:flex-row-reverse">
			<img
				src={pinkRoom}
				alt="Pink themed pet room"
				class="h-32 rounded border-text object-cover"
			/>

			<div class="text-center md:text-left">
				<h3 class="mb-2 text-2xl">Customize their room</h3>
        <p class="text-lg">
          Decorate your pet’s room with
          <span class="text-accent-darker">furniture you can purchase and unlock</span>,
          shaping a space that fits your desktop and your companion’s personality.
        </p>
			</div>
		</div>
	</div>

	<div class="rounded border-2 border-text bg-accent p-4">
		<div class="flex flex-col items-center gap-2 md:flex-row">
			<img src={lavaArea} alt="Lava themed adventure area" class="h-20 rounded object-cover p-2" />

			<div class="text-center md:text-left">
				<h3 class="mb-2 text-2xl">Explore new zones</h3>
        <p class="text-lg">
          Send your pet on
          <span class="text-accent-darker">small adventures</span>,
          explore new areas, and take part in casual,
          <span class="text-accent-darker">timing-based battles</span>.
        </p>
			</div>
		</div>
	</div>
</section>
