<script lang="ts">
  import { onMount } from "svelte";

  const files = [
    "capacitor",
    "capacitor2",
    "fluke1",
    "fluke2",
    "hall-effect",
    "LeftHandRule",
    "magnetic_fields",
    "mostek",
    "resistor",
  ];

  onMount(() => {
    const url = new URL(location.href);
    const tool = url.searchParams.get("program") ?? "capacitor";
    if (!files.includes(tool)) {
      url.searchParams.delete("program");
      location.href = url.toString();
      return;
    }

    // @ts-ignore
    const player = window.RufflePlayer.newest().createPlayer();
    document
      .querySelector(".wrapper")
      .replaceChild(player, document.querySelector(".player"));
    player.classList.toggle("player");
    player.style.width = "100%";
    player.style.height = "40rem";
    player.load(`/assets/${tool}.swf`).then(() => {
      player.play();
    });
  });

  function getToolURL(tool: string): string {
    const url = new URL(location.href);

    if (!files.includes(tool)) {
      url.searchParams.delete("program");
      return url.toString();
    }

    url.searchParams.set("program", tool);
    return url.toString();
  }
</script>

<main>
  <h1>PEiM player</h1>
  <section class="wrapper">
    <nav class="nav">
      <ul class="nav-list">
        {#each files as tool}
          <li class="nav-item">
            <a href={getToolURL(tool)}>{tool}</a>
          </li>
        {/each}
      </ul>
    </nav>
    <div class="player">
      <p>
        Something went wrong. Make sure that your browser supports WASM, you
        have JS enabled and that URL contains valid program name.
      </p>
    </div>
  </section>
  <footer>
    Made by <a href="https://github.com/KanarekLife/">Stanisław Nieradko</a>.
  </footer>
</main>
