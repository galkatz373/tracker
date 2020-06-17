<script>
  import { chart } from "svelte-apexcharts";
  // import Chart from "./../../components/Chart.svelte";
  import { numberWithCommas } from "./../../globals.js";
  import axios from "axios";
  import { onMount, afterUpdate } from "svelte";
  import { params } from "@sveltech/routify";
  import moment from "moment";

  export let name;

  let labels = [];
  let datasets = [];

  let cases = [];
  let deaths = [];
  let recovered = [];

  let currentDetails;

  onMount(() => {
    axios.get("https://corona.lmao.ninja/v2/countries/" + name).then((res) => {
      currentDetails = res.data;
    });

    axios
      .get(`https://corona.lmao.ninja/v2/historical/${name}?lastdays=40`)
      .then((res) => {
        Object.keys(res.data.timeline.cases).forEach((key) => {
          labels = [...labels, moment(key, "M/DD/YY").format("DD/MM/YY")];
          cases = [...cases, res.data.timeline.cases[key]];
        });

        Object.keys(res.data.timeline.deaths).forEach((key) => {
          deaths = [...deaths, res.data.timeline.deaths[key]];
        });

        Object.keys(res.data.timeline.recovered).forEach((key) => {
          recovered = [...recovered, res.data.timeline.recovered[key]];
        });
      });
  });
</script>

<style lang="scss">
  .flag {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    margin-bottom: 3rem;

    .name {
      font-weight: bold;
      font-size: 4rem;
      letter-spacing: 0.5mm;
      margin-bottom: 1rem;

      @media only screen and (max-width: 800px) {
        font-size: 2.5rem;
      }
    }

    img {
      height: 5rem;
      width: 7rem;
      border: 1px solid gray;

      @media only screen and (max-width: 800px) {
        height: 3rem;
        width: 4rem;
      }
    }
  }

  .chart-container {
    overflow-x: auto;
    display: flex;
    padding: 1rem;
    border: 1px solid lightgrey;
  }

  .details-container {
    margin-bottom: 2rem;
    display: flex;
    flex-wrap: wrap;
    justify-content: flex-start;

    .detail {
      display: flex;
      border-radius: 5px;
      border: 1px solid gray;
      margin: 1rem;
      text-align: center;
      padding: 1rem;
      font-weight: bold;
      justify-content: center;
      align-items: center;

      @media only screen and (max-width: 800px) {
        flex-basis: 38%;
        padding: 0.4rem 0.2rem;
      }
    }
  }

  .title {
    font-size: 2.2rem;
    font-weight: bold;

    @media only screen and (max-width: 800px) {
      font-size: 1.7rem;
    }
  }
</style>

{#if currentDetails}
  <div class="flag">
    <div class="name">{currentDetails.country}</div>
    <img
      alt="flag"
      src={`https://flagpedia.net/data/flags/h160/${name.toLowerCase()}.webp`} />
  </div>
  <div class="title">Stats</div>
  <div class="details-container">
    <div class="detail">{numberWithCommas(currentDetails.cases)} cases</div>
    <div class="detail">
      {numberWithCommas(currentDetails.todayCases)} new cases
    </div>
    <div class="detail">{numberWithCommas(currentDetails.deaths)} deaths</div>
    <div class="detail">
      {numberWithCommas(currentDetails.todayDeaths)} new deaths
    </div>
    <div class="detail">
      {numberWithCommas(currentDetails.critical)} critical cases
    </div>
    <div class="detail">
      {numberWithCommas(currentDetails.recovered)} recovered
    </div>
    <div class="detail">
      {numberWithCommas(currentDetails.active)} active cases
    </div>
    <div class="detail">
      {numberWithCommas(currentDetails.casesPerOneMillion)} cases per one
      million
    </div>
    <div class="detail">
      {numberWithCommas(currentDetails.deathsPerOneMillion)} deaths per one
      million
    </div>
    <div class="detail">{numberWithCommas(currentDetails.tests)} tests</div>
    <div class="detail">
      {numberWithCommas(currentDetails.testsPerOneMillion)} tests per one
      million
    </div>
    <div class="detail">
      {((currentDetails.deaths / currentDetails.cases) * 100).toFixed(2)}%
      mortality rate
    </div>
  </div>
{/if}
<div class="title">Chart</div>
<div
  class="chart-container"
  use:chart={{ chart: { type: 'line' }, tooltip: { y: { formatter: function (value, allData) {
          let firstValue = allData.series[allData.seriesIndex][allData.dataPointIndex - 1];
          if (firstValue) {
            return numberWithCommas(value) + ', ' + (value - firstValue) + ' from yesterday' + ', change of ' + (((value - firstValue) / value) * 100).toFixed(2) + '%';
          }
        } } }, dataLabels: { enabled: true, formatter: function (val, { seriesIndex, dataPointIndex, w }) {
        let firstValue = w.config.series[seriesIndex].data[dataPointIndex - 1];
        console.log(firstValue - val);
        if (firstValue - val > 0) {
          return '➘';
        } else if (firstValue === val) {
          return '-';
        } else {
          return '➚';
        }
        return numberWithCommas(val);
      } }, responsive: [{ breakpoint: undefined, options: {} }], colors: ['#6d6cc5', '#800080', '#ff0808', '#008000'], series: [{ name: 'cases', data: cases }, { name: 'active cases', data: cases.map((_, i) => cases[i] - deaths[i] - recovered[i]) }, { name: 'deaths', data: deaths }, { name: 'recovered', data: recovered }], xaxis: { categories: labels } }} />
