<script lang="ts">
	import './layout.css';
	import favicon from '$lib/assets/favicon.ico';
	import Navbar from '$lib/components/vpetlings/Navbar.svelte';
	import Footer from '$lib/components/vpetlings/Footer.svelte';

	let { children } = $props();
</script>

<svelte:head>
	<!-- Primary Meta -->
	<title>VPetlings · Wishlist on Steam!</title>
	<meta
		name="description"
		content="Wishlist now on Steam! Raise a cozy virtual pet that lives on your desktop. Hatch, care for, evolve, and battle your companion while you work, play, or browse."
	/>

	<!-- Favicon -->
	<link rel="icon" href={favicon} />

	<!-- Open Graph -->
	<meta property="og:type" content="website" />
	<meta property="og:title" content="VPetlings · Wishlist on Steam!" />
	<meta
		property="og:description"
		content="Wishlist now on Steam! Raise a cozy virtual pet that lives on your desktop. Hatch, care for, evolve, and battle your companion while you work, play, or browse."
	/>
	<meta property="og:image" content="/og-image.png" />
	<meta property="og:url" content="https://vpetlings.com" />

	<!-- Twitter / X -->
	<meta name="twitter:card" content="summary_large_image" />
	<meta name="twitter:title" content="VPetlings · Wishlist on Steam!" />
	<meta
		name="twitter:description"
		content="Wishlist now on Steam! Raise a cozy virtual pet that lives on your desktop. Hatch, care for, evolve, and battle your companion while you work, play, or browse."
	/>
	<meta name="twitter:image" content="/og-image.png" />
</svelte:head>

<div class="flex h-full min-h-screen flex-col bg-bg text-text bg-pattern">
	<Navbar />

	<main class="mx-auto w-full max-w-4xl flex-1 p-4 text-xl bg-[#fdfceb]/50">
		{@render children()}
	</main>

	<Footer />
</div>


<style>
    .bg-pattern {
        position: relative;
        z-index: 1;
    }

    .bg-pattern::before {
        content: "";
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        z-index: -1;
        background-position: center top;
        background-image: url('/src/lib/assets/background.webp');
        background-repeat: repeat-y;
        background-size: 1989px 1608px;
    }
</style>