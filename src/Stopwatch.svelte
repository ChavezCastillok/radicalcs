<script>
  let time = JSON.parse(localStorage.getItem("step")) ?? 0.0;
  let times = JSON.parse(localStorage.getItem("steps")) ?? [];
  let t;

  $: localStorage.setItem("step", JSON.stringify(time));
  $: localStorage.setItem("steps", JSON.stringify(times));

  const switchc = () => {
    if (!t) {
      t = setInterval(() => {
        time += 0.1;
      }, 100);
    } else {
      clearInterval(t);
      t = undefined;
    }
  };

  const restart = () => {
    times = [...times, time.toFixed("1")];
    time = 0.0;
  };

  const delete_times = () => {
    times = [];
  };
</script>

<section class="content">
  <h1>Cronometro</h1>
  <p>{time.toFixed("1")} seconds.</p>
  <button class="button" on:click={switchc}>play/stop</button>
  {#if time !== 0.0}
    <button class="button" on:click={restart}>restart</button>
  {/if}
  <article>
    <h1>Registro</h1>
    {#if times.length > 0}
      <button class="button is-danger is-outlined" on:click={delete_times}>
        delete
      </button>
    {/if}
    <ol>
      {#each times as t}
        <li>{t} seconds.</li>
      {/each}
    </ol>
  </article>
</section>
