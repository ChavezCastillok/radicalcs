<script lang="ts">
  import BtnBasicCalc from "./BtnBasicCalc.svelte";

  // pantallas
  let screenResult = "";
  let screenOperation = "";

  let calcPercentage = false;
  let parentesisOpen = false;

  // functions
  const eval_operation = (op: string): string => {
    const evalue = eval;
    try {
      let result = evalue(op);
      return String(result);
    } catch (error) {
      return "";
    }
  };

  function borrar() {
    if (String(screenOperation).length > 0) {
      screenOperation = String(screenOperation).slice(0, -1);
    }
  }

  function get_result() {
    let tempOp: string = screenOperation;
    if (String(tempOp).includes("x")) tempOp = tempOp.replaceAll("x", "*");
    if (String(tempOp).includes("%")) tempOp = tempOp.replaceAll("%", "/100");
    screenResult = eval_operation(tempOp);
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
      screenOperation += key;
    } else if (key == ".") {
      screenOperation != "" &&
      Number(screenOperation[screenOperation.length - 1]) != NaN
        ? (screenOperation += ".")
        : (screenOperation += "0.");
    } else if (key == "%") {
      screenOperation += "%";
      calcPercentage = true;
    } else if (key == "Backspace") borrar();
    else if (key == "c") screenOperation = "";
    else if (key == "Enter") screenOperation = screenResult;
  }

  $: screenOperation == "" ? (screenResult = "") : get_result();
</script>

<svelte:window on:keydown|preventDefault={handleKeydown} />

<!-- TODO
			agregar icons a los screenResult
		  al screen two cuando se escriba algo que aparezca el de borrar 
		-->
<div class="is-flex is-justify-content-center">
  <article class="box has-text-centered">
    <p class="control">
      <label class="label">
        <input
          class="input is-rounded"
          type="text"
          bind:value={screenOperation}
          placeholder="0.00"
          readonly
        />
      </label>
    </p>
    <p class="control">
      <label class="label">
        <input
          class="input is-rounded"
          type="text"
          bind:value={screenResult}
          placeholder="0.00"
          readonly
        />
      </label>
    </p>
    <p>
      <BtnBasicCalc val="C" bind:mathOperation={screenOperation} />
      <BtnBasicCalc
        val="⒩"
        bind:mathOperation={screenOperation}
        bind:parentesisOpen
      />
      <BtnBasicCalc
        val="%"
        bind:mathOperation={screenOperation}
        bind:calcPercentage
      />
      <BtnBasicCalc val="/" bind:mathOperation={screenOperation} />
    </p>
    <p>
      <BtnBasicCalc val="1" bind:mathOperation={screenOperation} />
      <BtnBasicCalc val="2" bind:mathOperation={screenOperation} />
      <BtnBasicCalc val="3" bind:mathOperation={screenOperation} />
      <BtnBasicCalc val="x" bind:mathOperation={screenOperation} />
    </p>

    <p>
      <BtnBasicCalc val="4" bind:mathOperation={screenOperation} />
      <BtnBasicCalc val="5" bind:mathOperation={screenOperation} />
      <BtnBasicCalc val="6" bind:mathOperation={screenOperation} />
      <BtnBasicCalc val="-" bind:mathOperation={screenOperation} />
    </p>
    <p>
      <BtnBasicCalc val="7" bind:mathOperation={screenOperation} />
      <BtnBasicCalc val="8" bind:mathOperation={screenOperation} />
      <BtnBasicCalc val="9" bind:mathOperation={screenOperation} />
      <BtnBasicCalc val="+" bind:mathOperation={screenOperation} />
    </p>
    <p>
      <BtnBasicCalc
        val="±"
        bind:mathOperation={screenOperation}
        bind:parentesisOpen
      />
      <BtnBasicCalc val="0" bind:mathOperation={screenOperation} />
      <BtnBasicCalc val="." bind:mathOperation={screenOperation} />
      <BtnBasicCalc
        val="="
        bind:mathOperation={screenOperation}
        bind:resultMathOperation={screenResult}
      />
    </p>
  </article>
</div>

<style>
  input {
    margin: 0.2rem;
    text-align: right;
    font-size: 1.2rem;
    width: 16rem;
    font-family: "Linux Biolinum G", "Linux Biolinum";
  }
</style>
