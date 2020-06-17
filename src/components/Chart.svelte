<script>
  import { afterUpdate } from "svelte";
  import { onMount } from "svelte";
  import Chart from "chart.js";
  export let labels = [];
  export let datasets = [];
  export let options = {};
  export let type = "line";

  let ctx;
  let myChart;

  onMount(() => {
    myChart = new Chart(ctx.getContext("2d"), {
      type: type,
      data: {
        labels: labels,
        datasets: datasets
      },
      options: options
    });
  });

  afterUpdate(() => {
    myChart.data.labels = labels;
    myChart.data.datasets = datasets;
    myChart.options = options;
    myChart.update();
  });
</script>

<style lang="scss">
  canvas {
    width: 100%;
    @media only screen and (max-width: 800px) {
      width: unset;
    }
  }
</style>

<canvas bind:this={ctx} width="1210" height="500" />
