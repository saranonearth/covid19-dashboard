<script>
  export let locationData;
  export let covidSummary;
  export let stateStat;
  let searchQuery = "";
  console.log(stateStat);
  let country = covidSummary.Countries.filter(
    e => e.Country == locationData.country
  )[0];
  let global = covidSummary.Global;
  let state = stateStat.statewise.filter(
    e => e.state === locationData.regionName
  )[0];
  console.log(locationData);
  console.log(covidSummary);
  console.log(global);
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
    color: rgb(196, 196, 196);
    border: none;
    margin-left: 10px;
    font-size: 1em;
    font-weight: 200 !important;
  }
  .search {
    font-size: 2em;
    padding: 15px;
    margin-top: 20px;
    border-radius: 8px;
    width: 720px;
    height: 40px;
    box-shadow: 0 5px 10px rgba(0, 0, 0, 0.12);
    display: flex;
  }
  .grey2 {
    font-size: 0.8em !important;
    color: rgb(196, 196, 196);
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
      <p>Total Cases:</p>
      <p class="grey c-stat">{state.confirmed}</p>
      <p class="head">Deaths</p>
      <p class="red c-stat">{state.deaths}</p>
      <p class="head">Recovered</p>
      <p class="green c-stat">{state.recovered}</p>
    </div>

  </div>
  <div class="search">
    <div>
      <i class="fa grey2 fa-search" aria-hidden="true" />
    </div>

    <div>

      <input
        type="text"
        bind:value={searchQuery}
        class="search-bar"
        placeholder="Search for a location" />

    </div>
  </div>
</main>
