<script lang="ts">
	import './layout.css';
	import favicon from '$lib/assets/favicon.ico';
	import Navbar from '$lib/components/vpetlings/Navbar.svelte';
	import Footer from '$lib/components/vpetlings/Footer.svelte';

	let { children } = $props();
</script>

<svelte:head>
	<!-- Primary Meta -->
	<title>VPetlings</title>
	<meta
		name="description"
		content="Wishlist now! Raise a cozy virtual pet that lives on your desktop. Hatch, care for, evolve, and battle your companion while you work, play, or browse."
	/>

	<!-- Favicon -->
	<link rel="icon" href={favicon} />

	<!-- Open Graph -->
	<meta property="og:type" content="website" />
	<meta property="og:title" content="VPetlings" />
	<meta
		property="og:description"
		content="A cozy desktop virtual pet inspired by Tamagotchi and Digimon. Raise, evolve, and care for your companion over time."
	/>
	<meta property="og:image" content="/og-image.png" />
	<meta property="og:url" content="https://vpetlings.com" />

	<!-- Twitter / X -->
	<meta name="twitter:card" content="summary_large_image" />
	<meta name="twitter:title" content="VPetlings - A Cozy Virtual Pet on Your Desktop" />
	<meta
		name="twitter:description"
		content="Raise your own virtual pet right on your desktop. A cozy companion while you work, play, or browse."
	/>
	<meta name="twitter:image" content="/og-image.png" />
</svelte:head>

<div class="flex h-full min-h-screen flex-col bg-bg text-text">
	<Navbar />

	<main class="mx-auto w-full max-w-6xl flex-1 p-2 text-xl">
		{@render children()}
	</main>

	<Footer />
</div>
