<script>
  import { numberWithCommas } from "./../globals.js";
  import axios from "axios";
  import { onMount } from "svelte";
  import Table from "../components/Table.svelte";
  import { isActive, url } from "@sveltech/routify";

  let rows = [];

  onMount(() => {
    axios.get("https://corona.lmao.ninja/v2/countries?sort=cases").then(res => {
      rows = res.data;
    });
  });

  let columns = [
    { Header: "flag", accessor: "flag", disableSortBy: true },
    { Header: "country", accessor: "name", disableSortBy: true },
    { Header: "total cases", accessor: "cases" },
    { Header: "today cases", accessor: "todayCases" },
    { Header: "deaths", accessor: "deaths" },
    { Header: "today deaths", accessor: "todayDeaths" },
    { Header: "critical cases", accessor: "critical" },
    { Header: "active cases", accessor: "active" },
    { Header: "cases per one million", accessor: "casesPerOneMillion" },
    { Header: "deaths per one million", accessor: "deathsPerOneMillion" },
    { Header: "recovered", accessor: "recovered" },
    { Header: "tests", accessor: "tests" },
    { Header: "tests per one million", accessor: "testsPerOneMillion" },
    { Header: "mortality rate", accessor: "mortalityRate" }
  ];
</script>

<style lang="scss">
  img {
    width: 2rem;
    height: 1.5rem;
  }
  .first-column {
    line-height: 50%;
  }

  td {
    font-size: 1.2rem;
    padding: 0.5rem 0;
    color: rgb(68, 66, 66);
    border-top: 1px solid lightgrey;
    border-bottom: 1px solid lightgrey;
    border-collapse: collapse;

    @media only screen and (max-width: 800px) {
      font-size: 0.8rem;
    }
  }
</style>

<Table
  rows={rows.map(row => ({
    ...row,
    mortalityRate: (row.deaths / row.cases) * 100
  }))}
  {columns}
  let:prop={row}>
  <td class="first-column">
    <a href={$url('/country/' + row.countryInfo.iso2)}>
      <img alt={row.country} src={row.countryInfo.flag} />
    </a>
  </td>
  <td>
    <a href={$url('/country/' + row.countryInfo.iso2)}>{row.country}</a>
  </td>
  <td>{numberWithCommas(row.cases)}</td>
  <td style="font-weight: bold">+{numberWithCommas(row.todayCases)}</td>
  <td>{numberWithCommas(row.deaths)}</td>
  <td style="font-weight: bold">+{numberWithCommas(row.todayDeaths)}</td>
  <td>{numberWithCommas(row.critical)}</td>
  <td>{numberWithCommas(row.active)}</td>
  <td>{numberWithCommas(row.casesPerOneMillion)}</td>
  <td>{numberWithCommas(row.deathsPerOneMillion)}</td>
  <td>{numberWithCommas(row.recovered)}</td>
  <td>{numberWithCommas(row.tests)}</td>
  <td>{numberWithCommas(row.testsPerOneMillion)}</td>
  <td>{((row.deaths / row.cases) * 100).toFixed(2)}%</td>
</Table>
