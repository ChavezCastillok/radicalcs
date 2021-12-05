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
    <ion-icon name="play" on:click={switchc} size='large'></ion-icon>
    <ion-icon name="stop" on:click={switchc} size='large'></ion-icon>
  {#if time !== 0.0}
  <ion-icon name="refresh" on:click={restart}  size='large'></ion-icon>
  {/if}
  <article>
    <h1>Registro</h1>
    <ol>
      {#each times as t}
        <li>{t} seconds.</li>
      {/each}
    </ol>
    {#if times.length > 0}
      <ion-icon name="trash" on:click={delete_times} size='large'></ion-icon>
    {/if}
  </article>
</section>
