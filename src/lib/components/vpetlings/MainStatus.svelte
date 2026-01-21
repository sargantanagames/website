<script>
import { LINKS } from "$lib/config/links.ts";
</script>

<section class="mx-auto max-w-2xl text-center space-y-4">
  <!-- Animated status line -->
  <div class="flex justify-center items-end text-3xl">
    <span>In development</span>
    <span class="build-dots">
        <span>.</span><span>.</span><span>.</span>
      </span>
  </div>

  <p class="text-xl">
    VPetlings is actively being worked on.
    <a
      href={LINKS.vpetlings.steam}
      target="_blank"
      rel="noopener noreferrer"
      class="underline-center"
    >
      <span class="text-accent-darker">Wishlist on Steam</span>
    </a>
    to follow development and support the project.
  </p>


  <!-- Steam widget -->
  <div class="flex justify-center">
    <iframe
      src="https://store.steampowered.com/widget/3799160/?t=Wishlist%20on%20Steam!"
      frameborder="0"
      width="646"
      height="190"
      loading="lazy"
      title="VPetlings Steam widget"
      class="max-w-full rounded"
    ></iframe>
  </div>

  <p class="pt-2 text-sm opacity-70">
    Want to learn more about us?
    <a
      href="/vpetlings/about-us"
      class="underline underline-offset-2 hover:opacity-80"
    >
      Read about the "studio"
    </a>.
  </p>
</section>

<style>
    .build-dots span {
        display: inline-block;
        animation: dot-jump 1s infinite ease-in-out both;
    }

    .build-dots span:nth-child(1) {
        animation-delay: 0s;
    }

    .build-dots span:nth-child(2) {
        animation-delay: 0.1s;
    }

    .build-dots span:nth-child(3) {
        animation-delay: 0.2s;
    }

    @keyframes dot-jump {
        0%,
        80%,
        100% {
            transform: translateY(0);
        }
        40% {
            transform: translateY(-2px);
        }
    }

    @media (prefers-reduced-motion: reduce) {
        .build-dots span {
            animation: none;
        }
    }

    .underline-center {
        position: relative;
        display: inline-block;
        text-decoration: none;
    }

    .underline-center::after {
        content: '';
        position: absolute;
        left: 50%;
        bottom: 4px;

        width: 0;
        height: 2px;

        background-color: var(--color-accent-darker);

        transform: translateX(-50%);
        transition: width 180ms ease-out;
    }

    .underline-center:hover::after {
        width: 100%;
    }

</style>
