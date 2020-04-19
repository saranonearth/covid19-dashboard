<script>
  import { onMount } from "svelte";
  import axios from "../node_modules/axios";
  import MainStat from "./components/MainStat.svelte";
  import News from "./components/News.svelte";
  let locationData = null;
  let covidSummary = null;
  let stateStat = null;
  let stateAll = null;
  onMount(async () => {
    const res = await axios.get("http://ip-api.com/json/");
    const covidRes = await axios.get("https://api.covid19api.com/summary");
    const stateRes = await axios.get("https://api.covid19india.org/data.json");
    const stateAllRes = await axios.get(
      "https://api.covid19india.org/state_district_wise.json"
    );
    stateStat = stateRes.data;
    covidSummary = covidRes.data;
    stateAll = Object.entries(stateAllRes.data);
    locationData = res.data;
    console.log(stateAll);
    console.log(locationData);
  });
</script>

<style>
  .container {
    width: 55%;
    margin: auto;
  }
  .header {
    margin-top: 150px;
  }
  .title {
    font-size: 4em;
    color: rgb(46, 46, 46);
  }
  .sub1 {
    color: rgb(189, 186, 186);
    font-size: 2.1em;
    margin-top: -20px;
    margin-left: 6px;
  }

  @media (max-width: 640px) {
    .container {
      width: 90%;
    }
  }
</style>

<main class="container">

  <div class="header">
    <h1 class="title">COVID-19</h1>
    <p class="sub1">Dashboard</p>

  </div>

  {#if locationData && covidSummary && stateStat && stateAll}
    <MainStat {locationData} {covidSummary} {stateStat} {stateAll} />
    <News {locationData} />
  {:else}
    <p>Loading..</p>
  {/if}

</main>
