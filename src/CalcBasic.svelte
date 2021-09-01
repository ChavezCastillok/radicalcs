<script>
  // pantallas
  let screen1 = "";
  let screen2 = "";

  let calc_percentage = false;
  let parentesisOpen = false;

  // functions
  const eval_operation = (op) => {
    let evalue = eval;
    try {
      let result = evalue(op);
      return result;
    } catch (error) {
      return "Invalid operation.";
    }
  };
  function pointClick() {
    if (screen2 != "" && Number(screen2[screen2.length - 1])) {
      screen2 += ".";
    } else {
      screen2 += "0.";
    }
  }

  function borrar() {
    if (String(screen2).length > 0) {
      screen2 = String(screen2).slice(0, -1);
    }
  }

  function get_result() {
    screen1 = screen2;
    screen2 = eval_operation(screen1);
    parentesisOpen = false;
  }

  function clean_screen() {
    screen1 = "";
    screen2 = "";
  }

  function do_mas_menos() {
    let negativo = false;
    if (screen2.length < 1 && !negativo) {
      screen2 += "-";
      negativo = true;
    } else if (screen2.length >= 1 && !negativo) {
      screen2 += "(-";
      parentesisOpen = true;
      negativo = true;
    } else {
      negativo = false;
    }
  }

  function handleKeydown(event) {
    let key = event.key;
    if (
      key == "1" ||
      key == "2" ||
      key == "3" ||
      key == "4" ||
      key == "5" ||
      key == "6" ||
      key == "7" ||
      key == "8" ||
      key == "9" ||
      key == "0" ||
      key == "+" ||
      key == "-" ||
      key == "*" ||
      key == "/" ||
      key == "(" ||
      key == ")"
    ) {
      screen2 += key;
    } else if (key == ".") pointClick();
    else if (key == "%") {
      screen2 += "%";
      calc_percentage = true;
    } else if (key == "Backspace") borrar();
    else if (key == "c") clean_screen();
    else if (key == "Enter") get_result();
  }

  $: if (calc_percentage) {
    if (screen2.length == 1) {
      screen1 = "100" + screen2;
      screen2 = "1";
    } else {
      let operation = screen2.replace("%", "");
      screen1 = `(${operation})%`;
      screen2 = eval_operation(operation) / 100;
    }
    calc_percentage = false;
  }
</script>

<svelte:window on:keydown|stopPropagation={handleKeydown} />

<section class="columns is-multiline">
  <article class="column is-flex is-justify-content-center">
    <table class="table">
      <caption>
        <h1 class="title">Calculadora</h1>
      </caption>
      <thead>
        <!-- TODO
          agregar icons a los screen1
        al screen two cuando se escriba algo que aparezca el de borrar 
      -->
        <tr>
          <th colspan="4">
            <input
              class="input"
              type="text"
              bind:value={screen1}
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
              bind:value={screen2}
              placeholder="0.00"
              readonly
            />
          </th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>
            <button class="button" on:click={clean_screen}>C</button>
          </td>
          <td
            ><button
              class="button"
              on:click={() => {
                if (parentesisOpen) {
                  screen2 += ")";
                  parentesisOpen = false;
                } else {
                  screen2 += "(";
                  parentesisOpen = true;
                }
              }}>⒩</button
            ></td
          >
          <td
            ><button
              class="button"
              on:click={() => {
                screen2 += "%";
                calc_percentage = true;
              }}>%</button
            ></td
          >

          <td>
            <button class="button" on:click={() => (screen2 += "/")}>
              /
            </button>
          </td>
        </tr>
        <tr>
          <td>
            <button class="button" on:click={() => (screen2 += "1")}>
              1
            </button>
          </td>
          <td>
            <button class="button" on:click={() => (screen2 += "2")}>
              2
            </button>
          </td>
          <td>
            <button class="button" on:click={() => (screen2 += "3")}>
              3
            </button>
          </td>
          <td
            ><button class="button" on:click={() => (screen2 += "*")}>
              x
            </button></td
          >
        </tr>
        <tr>
          <td>
            <button class="button" on:click={() => (screen2 += "4")}>
              4
            </button>
          </td>
          <td>
            <button class="button" on:click={() => (screen2 += "5")}>
              5
            </button>
          </td>
          <td>
            <button class="button" on:click={() => (screen2 += "6")}>
              6
            </button>
          </td>
          <td>
            <button class="button" on:click={() => (screen2 += "-")}>
              -
            </button>
          </td>
        </tr>
        <tr>
          <td>
            <button class="button" on:click={() => (screen2 += "7")}>
              7
            </button>
          </td>
          <td>
            <button class="button" on:click={() => (screen2 += "8")}>
              8
            </button>
          </td>
          <td>
            <button class="button" on:click={() => (screen2 += "9")}>
              9
            </button>
          </td>
          <td>
            <button class="button" on:click={() => (screen2 += "+")}>
              +
            </button>
          </td>
        </tr>
        <tr>
          <td>
            <button class="button" on:click={do_mas_menos}> ± </button>
          </td>
          <td>
            <button class="button" on:click={() => (screen2 += "0")}>
              0
            </button>
          </td>
          <td>
            <button class="button" on:click={pointClick}> . </button>
          </td>
          <td>
            <button class="button" on:click={get_result}> = </button>
          </td>
        </tr>
      </tbody>
    </table>
  </article>
</section>

<style>
  table {
    background-color: ghostwhite;
  }

  h1 {
    color: darkviolet;
  }

  input {
    text-align: right;
    font-size: 1.2rem;
    font-family: "Linux Biolinum G";
  }

  button {
    font-weight: bold;
    font-family: "Liberation Mono";
    color: darkviolet;
  }
</style>
