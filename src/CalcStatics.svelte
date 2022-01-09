<script>
  // todo
  let dato = {
    value: "",
    frequency: 1,
    accumulatedFrequency: 0,
  };
  let datos = [];
  let totalFrequency = 0;
  let totalValues = 0;
  let mediaAritmetica = 0;
  let mediaArmonica = 0;
  let mediaGeometrica = 0;
  let medianaSimple = 0;
  let mediana = 0;

  function add() {
    if (datos.length > 0) {
      dato.accumulatedFrequency =
        datos[datos.length - 1].accumulatedFrequency + dato.frequency;
    } else {
      dato.accumulatedFrequency = dato.frequency;
    }
    datos = [...datos, dato];
    totalFrequency += dato.frequency;
    totalValues += dato.value * dato.frequency;
    mediaAritmetica = totalValues / totalFrequency;
    mediaArmonica = calc_mediaArmonica();
    medianaSimple = calc_medianaSimple();
    mediana = calc_mediana();
    dato = { value: "", frequency: 1 };
  }

  const calc_mediaArmonica = () => {
    let h = 0;
    datos.forEach((dato) => {
      h += dato.frequency / dato.value;
    });
    return totalFrequency / h;
  };

  const calc_medianaSimple = () => {
    let list = [];
    datos.forEach((dato) => {
      list.push(parseFloat(dato.value));
    });
    list.sort((a, b) => a - b);
    if ((datos.length + 1) % 2 == 0) {
      return parseFloat(list[parseInt(list.length / 2)]);
    } else {
      let medioa = parseFloat(list[parseInt(list.length / 2) - 1]);
      let mediob = parseFloat(list[parseInt(list.length / 2)]);
      return parseFloat((medioa + mediob) / 2);
    }
  };

  const calc_mediana = () => {
    let limitInferior = 0;
    let interval = 0;
    return 123;
  };
</script>

<section class="section">
  <div class="notification is-warning">
    <button class="delete" />
    <strong>En desarrollo</strong>
  </div>
  <h1>Calculadora Estadistica</h1>
  <h2>Coming Soon</h2>

  <section>
    <table class="table">
      <tr>
        <td><input type="text" bind:value={dato.value} /></td>
        <td
          ><input
            type="number"
            min="0"
            size="6"
            bind:value={dato.frequency}
          /></td
        >
        <td>...</td>
        <td>
          <button class="button is-primary" on:click={add}> add </button>
        </td>
      </tr>
      <tr>
        <th>Value(X)</th>
        <th>Frequency(F)</th>
        <th>Accumulated Frequency(aF)</th>
        <th>Total(X*F)</th>
      </tr>
      {#each datos as dato}
        <tr>
          <td>{dato.value}</td>
          <td>{dato.frequency}</td>
          <td>{dato.accumulatedFrequency} </td>
          <td>{dato.value * dato.frequency}</td>
        </tr>
      {/each}
      <tr>
        <td>Totales:</td>
        <td>{totalFrequency}</td>
        <td>...</td>
        <td>{totalValues}</td>
      </tr>
    </table>
    <section class="columns">
      <section class="column">
        <article class="content">
          <h1>Medidas de tendencia central</h1>
          <p>Media aritmetica (X-): {mediaAritmetica.toFixed("2")}</p>
          <p>Media armonica (X-h): {mediaArmonica.toFixed("2")}</p>
          <p>Media Geométrica (X-g): {mediaGeometrica.toFixed("2")}</p>
          <p>
            Mediana simple (Mds): {medianaSimple.toFixed("2")} Clase (valor) que
            contiene la madiana.
          </p>
          <p>Mediana datos agrupados (Mdg): {mediana.toFixed("2")}</p>
        </article>
      </section>
      <section class="column">
        <article class="content">
          <h1>Desde pyodide</h1>
          <p>Por desarrollar...</p>
        </article>
      </section>
    </section>
  </section>
</section>
