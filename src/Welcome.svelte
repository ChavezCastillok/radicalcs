<script>
  let today = new Date();
  let born_date;
  let age_on_days;

  function go() {
    try {
      let time = today.getTime() - new Date(born_date).getTime();
      age_on_days = Math.round(time / (1000 * 60 * 60 * 24)) - 1;
    } catch (error) {
      console.log(error);
    }
  }
</script>

<section class="columns">
  <section class="column">
    <article class="content">
      <h1>Calculadoras</h1>
      <h2>Explora las pestañas</h2>
    </article>
  </section>
  <section class="column">
    <article class="box content">
      <h2 class="has-text-primary">Your age in days</h2>

      <p class="control is-flex is-justify-content-center">
        <label class="label" for="born_date">
          Born date
          <input
            class="input is-primary is-rounded is-focused"
            type="date"
            name="born_date"
            id="born_date"
            bind:value={born_date}
            on:change={go}
          /></label
        >
      </p>

      {#if age_on_days == undefined}
        <h3 class="has-text-success">Entry born date</h3>
      {:else if age_on_days == NaN}
        <h3>Entry born date correctly</h3>
      {:else}
        <p>
          You are <strong>{age_on_days}</strong> days old!
          <strong>Congratulations!</strong>
        </p>
        <h1 class="has-text-primary">
          Happy new day!
          <span class="icon">
            <ion-icon name="happy-outline" size="large" />
          </span>
        </h1>
      {/if}
    </article>
  </section>
</section>

<style>
  h1 {
    color: green;
  }
  h2 {
    color: darkviolet;
  }
</style>
