<script>
  import Stopwatch from "./Stopwatch.svelte";
  import Clock from "./Clock.svelte";
</script>

<section class="columns content">
  <article class="column" id="clock">
    <Clock />
    <hr>
    <p class="content">This clock was develop by Svelte team, and is available in the examples of your website. <a href="https://svelte.dev/examples#clock">Svelte.dev</a></p>
  </article>
  <article class="column">
    <Stopwatch />
  </article>
</section>

<style>
  #clock{
    max-width: 220px;
  }
</style>