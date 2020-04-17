<script>
  import { onMount } from "svelte";
  import axios from "axios";
  import MainStat from "./components/MainStat.svelte";
  import News from "./components/News.svelte";
  let locationData = localStorage.getItem("a")
    ? JSON.parse(localStorage.getItem("a"))
    : null;
  let covidSummary = localStorage.getItem("b")
    ? JSON.parse(localStorage.getItem("b"))
    : null;
  let stateStat = localStorage.getItem("c")
    ? JSON.parse(localStorage.getItem("c"))
    : null;
  onMount(async () => {
    const res = await axios.get("http://ip-api.com/json/");
    const covidRes = await axios.get("https://api.covid19api.com/summary");
    const stateRes = await axios.get("https://api.covid19india.org/data.json");
    stateStat = stateRes.data;
    covidSummary = covidRes.data;
    locationData = res.data;
    localStorage.setItem("a", JSON.stringify(res.data));
    localStorage.setItem("b", JSON.stringify(covidRes.data));
    localStorage.setItem("c", JSON.stringify(stateRes.data));
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
</style>

<main class="container">

  <div class="header">
    <h1 class="title">COVID-19</h1>
    <p class="sub1">Dashboard</p>

  </div>

  {#if locationData && covidSummary && stateStat}
    <MainStat {locationData} {covidSummary} {stateStat} />
    <News {locationData} />
  {:else}
    <p>Loading..</p>
  {/if}

</main>
