<script lang="ts">
  export let val = "";
  export let mathOperation = "";
  export let resultMathOperation = "";
  export let calcPercentage = false;
  export let parentesisOpen = false;

  function update_mathOperation(): void {
    if (val == "⒩") {
      if (parentesisOpen) {
        mathOperation += ")";
        parentesisOpen = false;
      } else {
        Number(mathOperation.split("")[mathOperation.length - 1])
          ? (mathOperation += "x(")
          : (mathOperation += "(");
        parentesisOpen = true;
      }
    } else if (val == ".") {
      mathOperation != "" &&
      Number(mathOperation[mathOperation.length - 1]) != NaN
        ? (mathOperation += ".")
        : (mathOperation += "0.");
    } else if (val == "C") {
      mathOperation = "";
    } else if (val == "%") {
      mathOperation += val;
      calcPercentage = true;
    } else if (val == "±") {
      let negativo = false;
      if (mathOperation.length < 1 && !negativo) {
        mathOperation += "-";
        negativo = true;
      } else if (mathOperation.length >= 1 && !negativo) {
        Number(mathOperation[mathOperation.length - 1]) != NaN
          ? (mathOperation += "x(-")
          : (mathOperation += "(-");
        parentesisOpen = true;
        negativo = true;
      } else {
        negativo = false;
      }
    } else if (val == "=") {
      mathOperation = resultMathOperation;
    } else {
      mathOperation += val;
    }
  }
</script>

<button class="button is-rounded" on:click={update_mathOperation}>
  {val}
</button>

<style>
  button {
    margin: 0.2rem;
    height: 3.3rem;
    width: 3.3rem;
    font-weight: bold;
    font-family: "Noto Mono", "monospace";
    color: darkviolet;
  }
</style>
