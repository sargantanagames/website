<script lang="ts">
  import vpetlingsLogo from "$lib/assets/vpetlings/vpetlings-logo.png";
  import capsuleImage from "$lib/assets/vpetlings/capsule.png";
  // import Pet from "$lib/components/vpetlings/Pet.svelte";
</script>

<section class="space-y-16">
  <div class="flex justify-center">
    <img src={vpetlingsLogo} alt="VPetlings" class="logo-bob h-24 object-contain md:h-32" />
  </div>

  <div class="flex flex-col md:flex-row gap-8 justify-between items-center text-center">
    <div>
      <h2 class="text-4xl">
        Your VPet <span class="text-accent-darker">on your desktop</span>!
      </h2>
      <p class="text-2xl">
        Choose your favorite, take care of them,
        play with them, <span class="text-accent-darker">train them for battle</span>,
        and help them <span class="text-accent-darker">evolve</span>!
        They will always be there, <span class="text-accent-darker">keeping you company</span>
        while you work, play, or browse.
      </p>
    </div>
    <img src={capsuleImage} alt="Steam Capsule" class="h-48 object-contain md:h-56" />
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
