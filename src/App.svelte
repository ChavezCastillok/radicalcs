<script lang="ts">
  import "bulma/css/bulma.css";
  import Welcome from "./Welcome.svelte";
  import BasicCalc from "./basicCalc/BasicCalc.svelte";
  import CalcDivisas from "./CalcDivisas.svelte";
  import CalcStatics from "./CalcStatics.svelte";
  import Footer from "./Footer.svelte";
  import CalcClock from "./CalcClock.svelte";

  let basic = false;
  let divisas = false;
  let statics = false;
  let oclock = false;

  function see_basic(): void {
    divisas = false;
    statics = false;
    oclock = false;
    basic = true;
  }
  function see_divisas() {
    basic = false;
    statics = false;
    oclock = false;
    divisas = true;
  }
  function see_statics() {
    basic = false;
    divisas = false;
    oclock = false;
    statics = true;
  }
  function see_oclock() {
    basic = false;
    divisas = false;
    statics = false;
    oclock = true;
  }
</script>

<main class="container">
  <div class="level">
    <section class="level-left">
      <header class="level-item">
        <h1 class="title">Radi<strong>Calcs</strong></h1>
      </header>
      <article class="level-item">
        <a href="index.html">Welcome</a>
      </article>
    </section>
    <section class="level-right">
      <article class="level-item">
        <nav class="tabs">
          <ul>
            <li class={basic ? "is-active" : ""} on:click={see_basic}>
              <!-- icon -->
              <!-- svelte-ignore a11y-missing-attribute -->
              <a>basic</a>
            </li>
            <li class={divisas ? "is-active" : ""} on:click={see_divisas}>
              <!-- icon -->
              <!-- svelte-ignore a11y-missing-attribute -->
              <a>divisas</a>
            </li>
            <li class={statics ? "is-active" : ""} on:click={see_statics}>
              <!-- icon -->
              <!-- svelte-ignore a11y-missing-attribute -->
              <a>statics</a>
            </li>
            <li class={oclock ? "is-active" : ""} on:click={see_oclock}>
              <!-- icon -->
              <!-- svelte-ignore a11y-missing-attribute -->
              <a>Reloj</a>
            </li>
          </ul>
        </nav>
      </article>
    </section>
  </div>
  <section class="section">
    {#if basic}
      <BasicCalc />
    {:else if divisas}
      <CalcDivisas />
    {:else if statics}
      <CalcStatics />
    {:else if oclock}
      <CalcClock />
    {:else}
      <Welcome />
    {/if}
  </section>
  <Footer />
</main>

<style>
  @font-face {
    font-family: "Comfortaa";
    src: url("../fonts/Comfortaa-Regular.ttf");
  }

  :global(main) {
    font-family: "Comfortaa";
  }
  a {
    color: green;
  }
  a:hover {
    color: darkgreen;
    text-decoration: none;
    text-transform: uppercase;
  }
  .title {
    color: darkviolet;
  }
  .tabs a {
    color: darkviolet;
  }
  .tabs a:hover {
    color: violet;
    text-decoration: none;
    text-transform: uppercase;
  }
  .tabs .is-active a {
    color: violet;
    font-weight: bold;
    text-transform: uppercase;
  }
  .section {
    text-align: center;
  }
</style>
