<script>
  let tasaCOP = JSON.parse(localStorage.getItem("tasaCOP")) || 3450;
  let tasaVES = JSON.parse(localStorage.getItem("tasaVES")) || 2360000;
  let montoUSD = "";
  let montoCOP = "";
  let montoVES = "";

  $: tasaVESCOP = (tasaCOP / tasaVES).toFixed("6");
  $: localStorage.setItem("tasaCOP", JSON.stringify(tasaCOP));
  $: localStorage.setItem("tasaVES", JSON.stringify(tasaVES));

  const calcularUSD = () => {
    if (montoUSD < 0) {
      montoUSD = 1;
    }
    montoVES = (montoUSD * tasaVES).toFixed("2");
    montoCOP = (montoUSD * tasaCOP).toFixed("2");
  };
  const calcularCOP = () => {
    montoVES = (montoCOP / tasaVESCOP).toFixed("2");
    montoUSD = (montoCOP / tasaCOP).toFixed("2");
  };
  const calcularVES = () => {
    montoCOP = (montoVES * tasaVESCOP).toFixed("2");
    montoUSD = (montoVES / tasaVES).toFixed("2");
  };

  const cleanMounts = () => {
    montoUSD = "";
    montoCOP = "";
    montoVES = "";
  };
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
      <div class="control mb-1">
        <div class="tags has-addons">
          <span class="tag is-dark">USD</span>
          <span class="tag is-success">Dolar de Estados Unidos</span>
        </div>
      </div>

      <div class="control mb-1">
        <div class="tags has-addons">
          <span class="tag is-dark">COP</span>
          <span class="tag is-success">Peso de Colombia</span>
        </div>
      </div>

      <div class="control mb-1">
        <div class="tags has-addons">
          <span class="tag is-dark">VES</span>
          <span class="tag is-success">Bolivar Soberano de Venezuela</span>
        </div>
      </div>
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
            bind:value={tasaCOP}
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
            bind:value={tasaVES}
            on:input={calcularUSD}
            placeholder="VES"
          />
        </label>
      </p>
      <p class="button is-static">VES</p>
    </div>
    <h5 class="subtitle">Tasa de cambio a VES</h5>

    <div class="field has-addons">
      <p class="control">
        <label class="label" for="cop">
          <input
            class="input"
            type="text"
            bind:value={tasaVESCOP}
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
