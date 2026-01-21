<script lang="ts">
  import vpetlingsLogo from "$lib/assets/vpetlings-logo.png";
  import SteamIcon from "$lib/components/SteamIcon.svelte";

  export let steamUrl: string;
</script>

<header class="header-dither w-full bg-accent-dark text-bg">
  <nav
    class="mx-auto flex max-w-5xl items-center gap-8 px-2"
  >

    <a href="/vpetlings" class="flex items-center">
      <img
        src={vpetlingsLogo}
        alt="VPetlings Logo"
        class="h-14 transition-transform ease-out hover:scale-[1.02]"
      />
    </a>

    <div class="flex-1"></div>

    <div class="flex items-center gap-6 text-2xl">
      <a
        href="/vpetlings/about-us"
        class="opacity-90 transition-opacity ease-out hover:opacity-100"
      >
        About Us
      </a>

      <a
        href="/vpetlings/press-kit"
        class="opacity-90 transition-opacity ease-out hover:opacity-100"
      >
        Press Kit
      </a>
    </div>


    <a
      href={steamUrl}
      target="_blank"
      rel="noopener noreferrer"
      class="
    inline-flex items-center gap-2
    rounded
    bg-gradient-to-r from-[#6b8f1a] to-[#4f6b0f]
    px-4 py-1
    text-sm text-[#d2efa9]
    font-light
    transition ease-out

    hover:from-[#7fa61e] hover:to-[#5c7e10]
  "
    >
      <SteamIcon classes="h-5" fill="white" />

    <span class="font-[Brisa_Sans]">Add to your wishlist</span>
    </a>
  </nav>
</header>

<style>
    .header-dither {
        position: relative;
    }

    .header-dither::after {
        content: "";
        position: absolute;
        left: 0;
        right: 0;
        bottom: -16px;
        height: 16px;

        background-image: url("/src/lib/assets/dither-checker-navbar.svg");
        background-repeat: repeat-x;
        background-size: 8px 12px;

        pointer-events: none;
    }
</style>