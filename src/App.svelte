<script>
  import Welcome from "./Welcome.svelte";

  import CalcBasic from "./CalcBasic.svelte";
  import CalcDivisas from "./CalcDivisas.svelte";
  import CalcNomina from "./CalcNomina.svelte";
  import CalcStatics from "./CalcStatics.svelte";
  import Footer from "./Footer.svelte";

  let basic = false;
  let divisas = false;
  let nomina = false;
  let statics = false;

  function see_basic() {
    divisas = false;
    nomina = false;
    statics = false;
    basic = true;
  }
  function see_divisas() {
    basic = false;
    nomina = false;
    statics = false;
    divisas = true;
  }
  function see_nomina() {
    basic = false;
    statics = false;
    divisas = false;
    nomina = true;
  }
  function see_statics() {
    basic = false;
    nomina = false;
    divisas = false;
    statics = true;
  }
</script>

<main class="container">
  <div class="hero is-fullheight has-text-centered">
    <header class="hero-head">
      <div class="level">
        <section class="level-left">
          <article class="level-item">
            <h1 class="title">Radi<strong>Calcs</strong></h1>
          </article>
          <article class="level-item">
            <a href="index.html">Welcome</a>
          </article>
        </section>
        <section class="level-right">
          <article class="level-item">
            <div class="tabs">
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
                <li class={nomina ? "is-active" : ""} on:click={see_nomina}>
                  <!-- icon -->
                  <!-- svelte-ignore a11y-missing-attribute -->
                  <a>nomina</a>
                </li>
                <li class={statics ? "is-active" : ""} on:click={see_statics}>
                  <!-- icon -->
                  <!-- svelte-ignore a11y-missing-attribute -->
                  <a>statics</a>
                </li>
              </ul>
            </div>
          </article>
        </section>
      </div>
    </header>
    <div class="hero-body is-justify-content-center">
      {#if basic}
        <CalcBasic />
      {:else if divisas}
        <CalcDivisas />
      {:else if nomina}
        <CalcNomina />
      {:else if statics}
        <CalcStatics />
      {:else}
        <Welcome />
      {/if}
    </div>
    <div class="hero-foot">
      <Footer />
    </div>
  </div>
</main>

<style>
  a {
    color: green;
  }
  a:hover {
    color: darkgreen;
    text-decoration: none;
    text-transform: uppercase;
  }
  main {
    background-color: ghostwhite;
    font-family: "Comfortaa";
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
  .hero-head {
    color: ghostwhite;
  }

  .hero-body {
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
</style>
