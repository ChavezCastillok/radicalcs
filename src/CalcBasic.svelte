<script>
  // pantallas
  let pantalla1 = "";
  let pantalla2 = "";

  // valores
  let operacion = null;
  let operador1 = null;
  let operador2 = null;

  // Events

  function pointClick() {
    if (pantalla2 != "") {
      pantalla2 += ".";
    } else {
      pantalla2 = "0.";
    }
  }

  function sumar() {
    operacion = "+";
    operador1 = obtener_operando();
    pantalla1 = `${operador1} ${operacion}`;
  }

  function restar() {
    operador1 = obtener_operando();
    if (operador1 == null || operador1 == "") {
      pantalla2 = "-";
    } else {
      operacion = "-";
      pantalla1 = `${operador1} ${operacion}`;
    }
  }

  function multiplicar() {
    operacion = "x";
    operador1 = obtener_operando();
    if (operador1 == "") {
      operador1 = 1;
    }
    pantalla1 = `${operador1} ${operacion}`;
  }

  function dividir() {
    operacion = "/";
    operador1 = obtener_operando();
    if (operador1 == "") {
      operador1 = 1;
    }
    pantalla1 = `${operador1} ${operacion}`;
  }

  function elevar() {
    operacion = "^";
    operador1 = obtener_operando();
    if (operador1 == "") {
      operador1 = 1;
    }
    pantalla1 = `${operador1} ${operacion}`;
  }

  // raiz
  function raizCuadrada() {
    operador1 = obtener_operando();
    if (operador1 == "") {
      pantalla2.placeholder = "First input number";
    } else {
      result = Math.sqrt(operador1);
      pantalla1 = "&radic;" + operador1;
      pantalla2 = result;
    }
  }

  // factorial
  function factorizar() {
    operador1 = obtener_operando();
    if (operador1 == "") {
      pantalla1 = "0!";
      pantalla2 = 1;
    } else {
      result = factorial(operador1);
      pantalla1 = operador1 + "!";
      pantalla2 = result;
    }
  }

  function result() {
    operador2 = pantalla2;
    pantalla1 = `${operador1} ${operacion} ${operador2}`;

    operador1 = parseFloat(operador1);
    operador2 = parseFloat(operador2);

    if (operacion == "+") {
      pantalla2 = operador1 + operador2;
    } else if (operacion == "-") {
      pantalla2 = operador1 - operador2;
    } else if (operacion == "x") {
      pantalla2 = operador1 * operador2;
    } else if (operacion == "/") {
      pantalla2 = operador1 / operador2;
    } else if (operacion == "^") {
      pantalla2 = Math.pow(operador1, operador2);
    }
  }

  function clean_screen() {
    pantalla1 = "";
    pantalla2 = "";
  }

  function obtener_operando() {
    let operando = pantalla2;
    pantalla2 = "";
    return operando;
  }

  function factorial(n) {
    if (n == 0 || n == 1) {
      return 1;
    } else if (n > 1) {
      return n * factorial(n - 1);
    }
  }

  function add_num(n) {
    pantalla2 += toString(n);
  }
</script>

<table class="table" id="calc">
  <caption>
    <h1 class="title">Calculadora</h1>
  </caption>
  <thead>
    <tr>
      <th colspan="4">
        <input
          class="input"
          type="text"
          bind:value={pantalla1}
          placeholder="0.00"
          readonly
        />
      </th>
    </tr>
    <tr>
      <th colspan="4">
        <input
          class="input"
          type="text"
          bind:value={pantalla2}
          placeholder="0.00"
          readonly
        />
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        <button class="button" on:click={raizCuadrada}>&radic;</button>
      </td>
      <td><button class="button" on:click={elevar}>^</button></td>
      <td><button class="button" on:click={factorizar}>!</button></td>
      <td><button class="button" on:click={clean_screen}>C</button></td>
    </tr>
    <tr>
      <td>
        <button
          class="button"
          on:click={() => {
            pantalla2 += "1";
          }}
        >
          1
        </button>
      </td>
      <td>
        <button
          class="button"
          on:click={() => {
            pantalla2 += "2";
          }}
        >
          2
        </button>
      </td>
      <td>
        <button
          class="button"
          on:click={() => {
            pantalla2 += "3";
          }}
        >
          3
        </button>
      </td>
      <td>
        <button class="button" on:click={dividir}> / </button>
      </td>
    </tr>
    <tr>
      <td>
        <button
          class="button"
          on:click={() => {
            pantalla2 += "4";
          }}
        >
          4
        </button>
      </td>
      <td>
        <button
          class="button"
          on:click={() => {
            pantalla2 += "5";
          }}
        >
          5
        </button>
      </td>
      <td>
        <button
          class="button"
          on:click={() => {
            pantalla2 += "6";
          }}
        >
          6
        </button>
      </td>
      <td>
        <button class="button" on:click={multiplicar}> x </button>
      </td>
    </tr>
    <tr>
      <td>
        <button
          class="button"
          on:click={() => {
            pantalla2 += "7";
          }}
        >
          7
        </button>
      </td>
      <td>
        <button
          class="button"
          on:click={() => {
            pantalla2 += "8";
          }}
        >
          8
        </button>
      </td>
      <td>
        <button
          class="button"
          on:click={() => {
            pantalla2 += "9";
          }}
        >
          9
        </button>
      </td>
      <td>
        <button class="button" on:click={restar}> - </button>
      </td>
    </tr>
    <tr>
      <td>
        <button
          class="button"
          on:click={() => {
            pantalla2 += "0";
          }}
        >
          0
        </button>
      </td>
      <td>
        <button class="button" on:click={pointClick}> . </button>
      </td>
      <td>
        <button class="button" on:click={result}> = </button>
      </td>
      <td>
        <button class="button" on:click={sumar}> + </button>
      </td>
    </tr>
  </tbody>
</table>

<style>
  #calc {
    background-color: ghostwhite;
  }

  h1 {
    color: darkviolet;
  }

  #calc input {
    text-align: right;
    font-size: 1.2rem;
    font-family: "Linux Biolinum G";
  }

  #calc button {
    font-weight: bold;
    font-family: "Liberation Mono";
    color: darkviolet;
  }
</style>
