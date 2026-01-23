<script lang="ts">
	import MainHero from '$lib/components/vpetlings/MainHero.svelte';
	import MainFeatures from '$lib/components/vpetlings/MainFeatures.svelte';
	import MainVisuals from '$lib/components/vpetlings/MainVisuals.svelte';
	import MainStatus from '$lib/components/vpetlings/MainStatus.svelte';
</script>

<div class="my-12 flex flex-col items-center gap-24 text-base">
	<MainHero />
	<MainFeatures />
	<MainVisuals />
	<MainStatus />
</div>
