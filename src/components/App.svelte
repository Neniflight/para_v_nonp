<script>
  import Graph from "./Graph.svelte"
  import Scroller from "@sveltejs/svelte-scroller";
  let count, index, offset, progress;
  let width, height;


</script>

<main>
  <Scroller
  top={0.0}
  bottom={1}
  threshold={0.5}
  bind:count
  bind:index
  bind:offset
  bind:progress
>
  <div class="background" slot="background" bind:clientWidth={width} bind:clientHeight={height}>

    <div class="progress-bars">
      <p>current section: <strong>{index + 1}/{count}</strong></p>
      <progress value={count ? (index + 1) / count : 0} />

      <p>total progress</p>
      <progress value={progress || 0} />
    </div>
  </div>
  <div class="foreground" slot="foreground">
    <section>
      <div class = "caption_text">Here we will sample people's incomes from different income distributions. The blue line represents what their true income distribution looks like.
      The red line is if we assumed their income distribution to be normal. The histogram shows a nonparametric approach to guessing the distribution of this person's income.
      </div>
      <Graph/> </section>
  </div>

  </Scroller>
</main>

<style>
  /* Write your CSS here */
  .background {
    width: 100%;
    height: 100vh;
    position: relative;
    outline: green solid 3px;
  }
  .caption_text{
    font-size: 20px; /* Adjust the font size as needed */
    color: #14407e; /* Change the color of the text */
    text-align: center; /* Align the text to the center */
    margin-top: 10px; /* Add some top margin for spacing */
}
  .foreground {
    width: 50%;
    margin: 0 auto;
    height: auto;
    position: relative;
    outline: red solid 3px;
  }

  .progress-bars {
    position: absolute;
    background: rgba(170, 51, 120, 0.2) /*  40% opaque */;
    visibility: visible;
  }

  section {
    height: 80vh;
    background-color: rgba(0, 0, 0, 0.2); /* 20% opaque */
    /* color: white; */
    outline: magenta solid 3px;
    text-align: center;
    max-width: 900px; /* adjust at will */
    color: black;
    padding: 1em;
    margin: 0 0 2em 0;
    display: flex;
    flex-direction: column;
    align-items: center; /* Center horizontally */
    text-align: center; /* Center text horizontally */
  }
</style>

