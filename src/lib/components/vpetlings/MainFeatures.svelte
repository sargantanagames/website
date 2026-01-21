<script lang="ts">
	import eggAnimation from '$lib/assets/egg-animation.gif';
	import pinkRoom from '$lib/assets/pink-room.gif';
	import lavaArea from '$lib/assets/lava-area.png';
</script>

<section class="max-w-96 space-y-12 md:max-w-3xl">
	<div class="rounded border-2 border-text bg-accent p-2">
		<div class="flex flex-col items-center gap-2 md:flex-row">
			<img
				src={eggAnimation}
				alt="Egg wobbing animation"
				class="h-24 w-24 rounded border-2 border-text bg-bg object-cover"
			/>

			<div class="text-center md:text-left">
				<h3 class="mb-2 text-xl">Hatch your egg</h3>
				<p>
					Hatch your egg and meet a tiny companion that lives on your desktop, always nearby as you
					go about your day
				</p>
			</div>
		</div>
	</div>

	<div class="rounded border-2 border-text bg-accent p-2">
		<div class="flex flex-col items-center gap-2 md:flex-row-reverse">
			<img
				src={pinkRoom}
				alt="Pink themed pet room"
				class="h-32 rounded border-text object-cover"
			/>

			<div class="text-center md:text-left">
				<h3 class="mb-2 text-xl">Customize their room</h3>
				<p class="opacity-80">
					Decorate your pet’s room with furniture you can purchase and unlock, shaping a space that
					fits your desktop and your companion’s personality
				</p>
			</div>
		</div>
	</div>

	<div class="rounded border-2 border-text bg-accent p-2">
		<div class="flex flex-col items-center gap-2 md:flex-row">
			<img src={lavaArea} alt="Lava themed adventure area" class="h-20 rounded object-cover p-2" />

			<div class="text-center md:text-left">
				<h3 class="mb-2 text-xl">Explore new zones</h3>
				<p class="opacity-80">
					Send your pet on small adventures, explore new areas, and take part in casual,
					timing-based battles
				</p>
			</div>
		</div>
	</div>
</section>
