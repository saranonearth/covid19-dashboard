<script>
  import Graphs from "./Graphs.svelte";

  export let locationData;
  export let covidSummary;
  export let stateStat;
  export let stateAll;

  let stateWiseData = null;
  let searchQuery = "";
  let searchData = null;
  let state = null;
  let countryName = locationData.country;
  let stateName = locationData.regionName;
  // console.log("stateAll", stateAll);
  // console.log("STATE STAT", stateStat);
  let country = covidSummary.Countries.filter(
    e => e.Country == locationData.country
  )[0];
  let global = covidSummary.Global;

  if (stateStat.statewise.filter(e => e.state === locationData.regionName)[0]) {
    state = stateStat.statewise.filter(
      e => e.state === locationData.regionName
    )[0];
  } else {
    state = null;
  }

  // console.log("LOCATION DATA", locationData);
  // console.log("COVID SUMMARY", covidSummary);
  // console.log("GLOBAL", global);

  let here = locationData.regionName;
  // console.log("HERE", here);

  if (
    stateAll.find(e => e[0].toLowerCase().trim() === here.toLowerCase().trim())
  ) {
    stateWiseData = stateAll.find(
      e => e[0].toLowerCase().trim() === here.toLowerCase().trim()
    );
  }

  // console.log("STATEWISE", stateWiseData);

  const handleSubmit = () => {
    const sData = covidSummary.Countries.filter(e => {
      if (e.Country.toLowerCase() === searchQuery.toLowerCase()) {
        return e.Country.toLowerCase() === searchQuery.toLowerCase();
      } else {
        return e.Country.toLowerCase()
          .split(" ")
          .join("")
          .includes(searchQuery.toLowerCase());
      }
    });
    if (sData.length > 0) {
      searchData = sData.reverse();
    } else {
      searchData = null;
    }

    searchQuery = "";
  };
  const detectMob = () => {
    return window.innerWidth <= 800 && window.innerHeight <= 600;
  };

  const handleTagClick = i => {
    const curState = stateAll[i];
    stateName = curState[0];
    stateWiseData = curState;
    if (detectMob) {
      window.scrollTo(window.pageYOffset + 500, window.pageYOffset + 500);
    }
  };

  // const handleCountryClick = c => {
  //   countryName = c.Country;
  // };
</script>

<style>
  .main-cont {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    margin-top: 20px;
  }

  .card {
    box-shadow: 0 5px 10px rgba(0, 0, 0, 0.12);
    width: 200px;
    padding: 20px;
    border-radius: 8px;
    margin-left: 10px;
  }

  .text-head {
    font-weight: 700;
    font-size: 1.2em;
    opacity: 0.7;
  }
  .grey {
    opacity: 0.5;
  }
  .ml-1 {
    margin-left: 10px;
  }
  .green {
    color: #8aca2b;
  }
  .red {
    color: rgb(241, 38, 38);
  }
  .c-stat {
    margin-top: -2px;
    font-size: 1.8em;
  }
  .search-bar {
    color: rgb(212, 212, 212);
    border: none;
    margin-left: 10px;
    font-size: 1em;
    font-weight: 200 !important;
  }
  .search {
    font-size: 1.7em;
    padding: 15px;
    margin-top: 20px;
    background: white;
    border-radius: 8px;
    width: 715px;
    height: 40px;
    box-shadow: 0 5px 10px rgba(0, 0, 0, 0.12);
    display: flex;
    margin-left: 10px;
    font-weight: 200 !important;
  }
  .grey2 {
    font-size: 0.8em !important;
    color: rgb(212, 212, 212);
  }
  .flex {
    display: flex;
    flex-direction: row;
  }
  .bo {
    width: 50%;
  }
  .so {
    width: 50%;
  }
  .stretch {
    width: 700px;
  }
  .search-data {
    margin-top: 15px;
    margin-bottom: 20px;
  }
  .in-reg {
    margin-top: 20px;
  }
  .mb-d {
    margin-bottom: 10px;
  }
  .cont-tab {
    margin-top: 10px;
    width: 50%;
    margin: auto;
  }
  .m {
    text-align: right;
  }
  .row {
    margin: 10px 0px;
    border-bottom: 0.9px solid rgb(221, 221, 221);
    padding-bottom: 8px;
  }
  .textb {
    font-weight: 600;
  }
  .row {
    color: rgb(180, 180, 180);
  }
  .tags {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
  }

  .tag {
    color: white;
    background: rgb(31, 30, 30);
    padding: 8px;
    font-size: 0.9em;
    margin-right: 5px;
    margin-top: 5px;
    border-radius: 8px;
    box-shadow: 0 5px 10px rgba(0, 0, 0, 0.12);
  }
  .tag:hover {
    cursor: pointer;
    opacity: 0.9;
  }
  @media (max-width: 640px) {
    .tag {
      font-size: 0.7em;
      padding: 8px;
    }
    .search {
      width: 265px;
      margin-left: -5px;
    }
    .card {
      width: 280px;
    }
    input {
      width: 200px !important;
      font-size: 0.8em !important;
    }
    input:focus,
    textarea:focus,
    select:focus {
      outline: none;
    }
    .search-bar {
      width: 250px;
      font-size: 1em;
    }
    .cont-tab {
      width: 90%;
    }
    .m {
      text-align: right;
    }
    .search-data {
      margin-left: -10px;
    }
    .main-cont {
      margin-left: -10px;
    }
  }
</style>

<main>
  <div class="main-cont">
    <div class="card">
      <p class="text-head">Global</p>
      <p>Total Cases:</p>
      <p class="grey c-stat">{global.TotalConfirmed}</p>
      <p class="head">Deaths</p>
      <p class="red c-stat">{global.TotalDeaths}</p>
      <p class="head">Recovered</p>
      <p class="green c-stat">{global.TotalRecovered}</p>
    </div>
    <div class="card">
      <p class="text-head">{locationData.country}</p>
      <p>Total Cases:</p>
      <p class="grey c-stat">{country.TotalConfirmed}</p>
      <p class="head">Deaths</p>
      <p class="red c-stat">{country.TotalDeaths}</p>
      <p class="head">Recovered</p>
      <p class="green c-stat">{country.TotalRecovered}</p>
    </div>
    <div class="card">
      <p class="text-head">{locationData.regionName}</p>
      {#if state}
        <p>Total Cases:</p>
        <p class="grey c-stat">{state.confirmed}</p>
        <p class="head">Deaths</p>
        <p class="red c-stat">{state.deaths}</p>
        <p class="head">Recovered</p>
        <p class="green c-stat">{state.recovered}</p>
      {:else}
        <p>No data found.</p>
      {/if}
    </div>

  </div>
  <div class="search">
    <div>
      <i class="fa grey2 fa-search" aria-hidden="true" />
    </div>

    <div>

      <form on:submit|preventDefault={handleSubmit}>
        <input
          type="text"
          bind:value={searchQuery}
          class="search-bar"
          placeholder="Search for a country" />
      </form>
    </div>
  </div>
  <div class="search-data main-cont">
    {#if searchData}
      <h2 class="ml-1 mb-d">Similar Results</h2>
      {#each searchData as c}
        <div class="card stretch">
          <h3>{c.Country}</h3>
          <div class="flex">

            <div class="bo">
              <p>Total Cases:</p>
              <p class="grey c-stat">{c.TotalConfirmed}</p>
              <p class="head">Deaths</p>
              <p class="red c-stat">{c.TotalDeaths}</p>
              <p class="head">Recovered</p>
              <p class="green c-stat">{c.TotalRecovered}</p>
            </div>
            <div class="so">
              <p>New Cases:</p>
              <p class="grey c-stat">{c.NewConfirmed}</p>
              <p class="head">New Deaths</p>
              <p class="red c-stat">{c.NewDeaths}</p>
              <p class="head">New Recovered</p>
              <p class="green c-stat">{c.NewRecovered}</p>
            </div>
          </div>
        </div>
      {/each}
    {/if}

  </div>
  <h2 class="ml-1 mb-d">In your country</h2>
  <Graphs country={countryName} />
  <div class="main-cont">
    <div class="tags">
      {#if stateWiseData}
        {#each stateAll as state, i}
          <div class="tag" on:click={() => handleTagClick(i)}>
            <p>{state[0]}</p>
          </div>
        {/each}
      {/if}
    </div>
  </div>
  <div class="in-reg main-cont">
    <h2 class="ml-1">In {stateName}</h2>
    <div class="card in-reg stretch">
      <div class="cont-tab">

        <div class="flex row">
          <div class="bo">
            <p class="textb">District</p>
          </div>
          <div class="so m">
            <p class="textb">Confirmed Cases</p>
          </div>
        </div>
      </div>
      {#if stateWiseData}
        {#each Object.entries(stateWiseData[1].districtData) as a}
          <div class="cont-tab">
            <div class="flex row">
              <div class="bo">
                <p>{a[0]}</p>
              </div>
              <div class="so m">
                <p>{a[1].confirmed}</p>
              </div>
            </div>
          </div>
        {/each}
      {:else}
        <center>
          <p>No data found</p>
        </center>
      {/if}
    </div>
  </div>
</main>
