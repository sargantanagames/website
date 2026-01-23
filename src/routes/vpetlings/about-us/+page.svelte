<script lang="ts">
	import diegoPhoto from '$lib/assets/diego.jpeg';
	import gerardPhoto from '$lib/assets/gerard.jpeg';
</script>

<section class="mx-auto my-24 max-w-4xl space-y-16 text-center">
	<p class="mx-auto max-w-3xl text-2xl">
		We’re just
		<span class="text-accent-darker">two friends making games</span>. Not really a studio. We’re
		from the same hometown, but now live in different cities, so we don’t see each other that often.
		When we do, we usually go for a run.
	</p>

	<div class="grid gap-12 md:grid-cols-2">
		<!-- Diego -->
		<div>
			<div class="relative mx-auto w-fit">
				<img
					src={diegoPhoto}
					alt="Diego"
					class="h-48 w-48 rounded border-2 border-text object-cover"
				/>

				<h3
					class="
            absolute -bottom-3 left-1/2
            -translate-x-1/2
            rounded border-2 border-text
            bg-accent
            px-3
            text-xl
            whitespace-nowrap
          "
				>
					Diego
				</h3>
			</div>

			<p class="mt-6 text-xl">
				<span class="text-accent-darker">Programming & Game Design.</span>
				Software developer by day, game developer by passion. Takes care of the code, systems, website,
				and
				<span class="text-accent-darker">everything that makes the game work</span>.
			</p>
		</div>

		<!-- Gerard -->
		<div>
			<div class="relative mx-auto w-fit">
				<img
					src={gerardPhoto}
					alt="Gerard"
					class="h-48 w-48 rounded border-2 border-text object-cover"
				/>

				<h3
					class="
            absolute -bottom-3 left-1/2
            -translate-x-1/2
            rounded border-2 border-text
            bg-accent
            px-2
            text-xl
            whitespace-nowrap
          "
				>
					Gerard
				</h3>
			</div>

			<p class="mt-6 text-xl">
				<span class="text-accent-darker">Art & Game Design.</span>
				Studied industrial design, but has always been fond of drawing.
				<span class="text-accent-darker">If you can see it, he made it</span>. Also responsible for
				the
				<span class="text-accent-darker">marketing and social media</span>.
			</p>
		</div>
	</div>
</section>
