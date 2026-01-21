<script lang="ts">
	import vpetlingsLogo from '$lib/assets/vpetlings-logo.png';
  import Pet from "$lib/components/vpetlings/Pet.svelte";
</script>

<section class="text-center">
	<div class="flex justify-center">
		<img src={vpetlingsLogo} alt="VPetlings" class="logo-bob h-24 object-contain md:h-28" />
	</div>

	<div class="relative mx-auto mt-12 h-48 w-48">
		<p
			class="
        absolute -top-4 left-1/2 z-10
        w-fit
        -translate-x-1/2 rounded border-2
        border-text bg-accent
        px-3
        py-1
        text-base whitespace-nowrap
        sm:text-xl
      "
		>
			A virtual pet that <span class="text-accent-darker">lives on your desktop</span>

    </p>

    <div
      class="h-full w-full rounded border-2 border-text bg-black/20"
    >
      <Pet />
    </div>
</section>

<style>
	.logo-bob {
		animation: bob 2s ease-in-out infinite;
		transform-origin: center;
	}

	@keyframes bob {
		0% {
			transform: translateY(0);
		}
		50% {
			transform: translateY(-8px);
		}
		100% {
			transform: translateY(0);
		}
	}

	@media (prefers-reduced-motion: reduce) {
		.logo-bob {
			animation: none;
		}
	}
</style>
