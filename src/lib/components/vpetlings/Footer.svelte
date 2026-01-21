<script lang="ts">
  import { LINKS } from "$lib/config/links";
</script>

<footer class="footer-dither w-full text-bg">
  <div class="mx-auto max-w-5xl p-2">
    <div class="flex flex-col items-center gap-2 text-center">
      <p>
        SargantanaGames © {new Date().getFullYear()}
      </p>

      <div class="flex items-center gap-6">
        <a
          href={LINKS.vpetlings.steam}
          target="_blank"
          rel="noopener noreferrer"
          class="
      opacity-80 transition-all ease-out
      hover:text-gray-500 hover:opacity-100
    "
        >
          Steam
        </a>

        <a
          href={LINKS.vpetlings.twitter}
          target="_blank"
          rel="noopener noreferrer"
          class="
      opacity-80 transition-all ease-out
      hover:text-cyan-400 hover:opacity-100
    "
        >
          Twitter
        </a>

        <a
          href={LINKS.vpetlings.instagram}
          target="_blank"
          rel="noopener noreferrer"
          class="
      opacity-80 transition-all ease-out
      hover:text-pink-400 hover:opacity-100
    "
        >
          Instagram
        </a>
      </div>
    </div>
  </div>
</footer>

<style>
    .footer-dither {
        position: relative;
        background: var(--color-secondary);
    }

    .footer-dither::before {
        content: '';
        position: absolute;
        left: 0;
        right: 0;
        top: -12px;
        height: 12px;

        background-image: url('/src/lib/assets/dither-checker-footer.svg');
        background-repeat: repeat-x;
        background-size: 8px 12px;

        pointer-events: none;
    }
</style>
