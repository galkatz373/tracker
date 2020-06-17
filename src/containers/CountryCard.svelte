<script>
  import { numberWithCommas } from "./../globals.js";
  import { goto } from "@sveltech/routify";
  import axios from "axios";
  import { onMount } from "svelte";
  export let details;

  // let countryData = [];
  // onMount(() => {
  //   axios
  //     .get(`https://corona.lmao.ninja/v2/countries/${countryCode}`)
  //     .then(res => {
  //       console.log(res.data);
  //       countryData = res.data;
  //     });
  // });
</script>

<style lang="scss">
  .container {
    flex-grow: 2;
    width: 10rem;
    border-radius: 5px;
    margin: 2rem;
    justify-content: center;
    align-items: center;
    text-align: center;
    padding: 1rem 2rem;

    @media only screen and (max-width: 1200px) {
      border-width: 1px;
      padding: 0 0.5rem;
    }

    .name {
      font-weight: bold;
      margin-bottom: 1rem;
    }

    .detail {
      font-weight: bold;
      border-radius: 5px;
      padding: 1rem 1rem;
      font-size: 1rem;
      margin: 1rem 0;
      word-break: normal;

      span {
        color: whitesmoke;
      }
    }
  }

  img {
    height: 4rem;
    width: 4.5rem;
    border-radius: 50%;
    border: 1px solid lightgrey;
    cursor: pointer;
  }
</style>

{#if details}
  <div class="container">
    <div class="name">{details.country}</div>
    <img
      alt={details.country}
      on:click={() => $goto('/country/' + details.countryInfo.iso2)}
      src={details.countryInfo.flag} />
    <div class="detail" style="background-color: #6d6cc5">
      <span>{numberWithCommas(details.cases)} {' '} cases</span>
      <br />
      <span>{numberWithCommas(details.todayCases)} today</span>
    </div>
    <div class="detail" style="background-color: red">
      <span>{numberWithCommas(details.deaths)} {' '} deaths</span>
      <br />
      <span>{numberWithCommas(details.todayDeaths)} today</span>
    </div>
    <div class="detail" style="background-color: #429028">
      <span>{numberWithCommas(details.recovered)} {' '} recovered</span>
    </div>
  </div>
{/if}
