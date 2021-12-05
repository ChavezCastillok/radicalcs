<script lang="ts">
  import { onMount } from "svelte";

  let exchangeRateBCV: number = null;

  let rateCOP: number = JSON.parse(localStorage.getItem("rateCOP")) ?? 3500.0;
  let rateVES: number =
    JSON.parse(localStorage.getItem("rateVES")) ?? exchangeRateBCV;
  let montoUSD: number;
  let montoCOP: number;
  let montoVES: number;

  const divisas = [
    { siglas: "USD", description: "Dolar de Estados Unidos." },
    { siglas: "COP", description: "Peso de Colombia" },
    { siglas: "VED", description: 'Bolivar "digital" de Venezuela' },
  ];

  $: rateVESCOP = Number((rateCOP / rateVES).toFixed(2));
  $: localStorage.setItem("rateCOP", JSON.stringify(rateCOP));
  $: localStorage.setItem("rateVES", JSON.stringify(rateVES));

  function calcularUSD() {
    montoVES = Number((montoUSD * rateVES).toFixed(2));
    montoCOP = Number((montoUSD * rateCOP).toFixed(2));
  }
  function calcularCOP() {
    montoVES = Number((montoCOP / rateVESCOP).toFixed(2));
    montoUSD = Number((montoCOP / rateCOP).toFixed(2));
  }
  function calcularVES() {
    montoCOP = Number((montoVES * rateVESCOP).toFixed(2));
    montoUSD = Number((montoVES / rateVES).toFixed(2));
  }

  function cleanMounts() {
    montoUSD = null;
    montoCOP = null;
    montoVES = null;
  }

  async function dolarBCV() {
    await fetch("https://api-divisas-ve.herokuapp.com/v1/")
      .then((res) => res.json())
      .then((json) => {
        exchangeRateBCV = parseFloat(json.data.dollar.value);
      })
      .catch((error) => console.log(error));
  }

  onMount(() => {
    dolarBCV();
  });
</script>

<div class="columns is-multiline">
  <div class="column is-full">
    <header>
      <h1 class="title">Calculadora de divisas</h1>
    </header>
  </div>
  <div class="column">
    <h5 class="title is-5">Equivalencias</h5>
    <div class="field has-addons">
      <p class="control">
        <label class="label" for="cop">
          <input
            class="input"
            type="text"
            bind:value={montoUSD}
            on:input={calcularUSD}
            on:click={cleanMounts}
            placeholder="0.00"
          />
        </label>
      </p>
      <p class="button is-static">USD</p>
    </div>

    <div class="field has-addons">
      <p class="control">
        <label class="label" for="cop">
          <input
            class="input"
            type="text"
            bind:value={montoCOP}
            on:click={cleanMounts}
            on:input={calcularCOP}
            placeholder="0.00"
          />
        </label>
      </p>
      <p class="button is-static">COP</p>
    </div>

    <div class="field has-addons">
      <p class="control">
        <label class="label" for="cop">
          <input
            class="input"
            type="text"
            on:click={cleanMounts}
            bind:value={montoVES}
            on:input={calcularVES}
            placeholder="0.00"
          />
        </label>
      </p>
      <p class="button is-static">VES</p>
    </div>

    <div>
      {#each divisas as moneda}
        <div class="control mb-1">
          <div class="tags has-addons">
            <span class="tag is-dark">{moneda.siglas}</span>
            <span class="tag is-success">{moneda.description}</span>
          </div>
        </div>
      {/each}
    </div>
  </div>
  <div class="column">
    <h5 class="title is-5">Referencias</h5>
    <h5 class="subtitle">Tasa de cambio a USD</h5>
    <div class="field has-addons">
      <p class="control">
        <label class="label" for="cop">
          <input
            class="input"
            type="text"
            bind:value={rateCOP}
            on:input={calcularUSD}
            placeholder="COP"
          />
        </label>
      </p>
      <p class="button is-static">COP</p>
    </div>
    <div class="field has-addons">
      <p class="control">
        <label class="label" for="cop">
          <input
            class="input"
            type="text"
            bind:value={rateVES}
            on:input={cleanMounts}
            placeholder="VES"
          />
        </label>
      </p>
      <p class="button is-static">VES</p>
    </div>

    <div>
      {#if rateVES == exchangeRateBCV}
      <p>Tasa BCV <span class="tag is-warning">en uso</span></p>
        
      {:else if exchangeRateBCV == null}
        <p class="help is-info">Tasa BCV no disponible.</p>
      {:else}
        <p class="help is-link">
          Tasa BCV: {exchangeRateBCV}
          {#if rateVES != exchangeRateBCV}
            <span class="tag is-success"
            on:click={()=>rateVES = exchangeRateBCV}
            >
              usar
            </span>
          {/if}
        </p>
        <!-- <button
          class="button"
          title={String(exchangeRateBCV) || "usar"}
          on:click={() => {
            rateVES = exchangeRateBCV;
          }}
        >
          Tasa BCV
        </button> -->
      {/if}
    </div>

    <h5 class="subtitle">Tasa de cambio a VES</h5>

    <div class="field has-addons">
      <p class="control">
        <label class="label" for="cop">
          <input
            class="input"
            type="text"
            bind:value={rateVESCOP}
            placeholder="VESCOP"
            readonly
          />
        </label>
      </p>
      <p class="button is-static">COP</p>
    </div>
  </div>
</div>

<style>
  .columns {
    justify-content: center;
  }
  .column {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
  .input {
    font-family: "Linux Biolinum G", "Linux Biolinum";
    text-align: right;
  }
  .button.is-static {
    font-family: "Liberation Mono";
  }
</style>
