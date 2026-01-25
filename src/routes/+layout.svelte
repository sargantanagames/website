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

<div class="bg-pattern flex h-full min-h-screen flex-col bg-bg text-text">
	<Navbar />

	<main class="mx-auto w-full max-w-4xl flex-1 bg-[#fdfceb]/50 p-4 text-xl">
		{@render children()}
	</main>

	<Footer />
</div>

<style>
	.bg-pattern {
		position: relative;
		z-index: 1;
	}

  .bg-pattern {
      background-image: url('/src/lib/assets/background.png');
      background-repeat: repeat-y;
      background-position: center top;
      background-size: 2040px 1608px;
      min-height: 100vh;
  }

</style>
