<script>
  import { onMount } from "svelte";
  import axios from "../node_modules/axios";
  import MainStat from "./components/MainStat.svelte";
  import News from "./components/News.svelte";
  import publicIp from "public-ip";

  let locationData = null;
  let covidSummary = null;
  let stateStat = null;
  let stateAll = null;
  onMount(async () => {
    const ip = await publicIp.v4();
    const res = await axios.get(`https://ipapi.co/${ip}/json/`);
    const covidRes = await axios.get("https://api.covid19api.com/summary");
    const stateRes = await axios.get("https://api.covid19india.org/data.json");
    const stateAllRes = await axios.get(
      "https://api.covid19india.org/state_district_wise.json"
    );

    stateStat = stateRes.data;
    covidSummary = covidRes.data;
    stateAll = Object.entries(stateAllRes.data);
    locationData = {
      countryCode: res.data.country,
      regionName: res.data.region,
      country: res.data.country_name
    };
    // console.log("A", stateAll);
    // console.log("B", locationData);
    // console.log("C", covidSummary);
    // console.log("D", stateStat);

    window.scrollTo(0, 0);
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
  .footer {
    margin-top: 50px;
    text-align: center;
    padding: 20px;
  }
  .icon {
    font-size: 1.3em !important;
    transition: 0.2s;
  }
  .icon:hover {
    color: rgb(83, 83, 83);
    cursor: pointer;
  }

  .loading {
    margin-top: 20px;
    height: 0;
    width: 0;
    padding: 15px;
    border: 6px solid #ccc;
    border-right-color: #888;
    border-radius: 22px;
    -webkit-animation: rotate 1s infinite linear;
    position: absolute;
    left: 50%;
    top: 50%;
  }
  a {
    color: rgb(68, 68, 68);
  }
  @-webkit-keyframes rotate {
    100% {
      -webkit-transform: rotate(360deg);
    }
  }
  @media (max-width: 640px) {
    .container {
      width: 90%;
    }
    .title {
      font-size: 3em;
    }
    .sub1 {
      font-size: 1.8em;
    }
  }
</style>

<main class="container">

  <div class="header">
    <h1 class="title">COVID-19</h1>
    <p class="sub1">Dashboard</p>

    {#if locationData && covidSummary && stateStat && stateAll}
      <MainStat {locationData} {covidSummary} {stateStat} {stateAll} />
      <!-- <News {locationData} /> -->
      <div class="footer">
        <div>

          <a
            class="icon"
            href="https://github.com/saranonearth/covid19-dashboard">
            <i class="fab icon fa-github" />
          </a>

        </div>

      </div>
    {:else}
      <div class="loading" />
    {/if}

  </div>

</main>
