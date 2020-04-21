<script>
  import { onMount } from "svelte";
  export let country;
  let res = null;
  let res2 = null;
  let res3 = null;
  let ctx = null;
  let ctx2 = null;
  onMount(async () => {
    res = await fetch(
      `https://api.covid19api.com/dayone/country/${country}/status/confirmed`
    );
    res2 = await fetch(
      `https://api.covid19api.com/dayone/country/${country}/status/deaths`
    );
    res3 = await fetch(
      `https://api.covid19api.com/dayone/country/${country}/status/recovered`
    );

    const data = await res.json();
    const data2 = await res2.json();
    const data3 = await res3.json();
    const dates = [];
    const cases = [];

    const cases2 = [];
    const cases3 = [];
    data.map(e => {
      dates.push(dateFns.format(new Date(e.Date), "MMM/DD"));
      //   console.log(new Date(e.Date));
      cases.push(e.Cases);
    });
    data2.map(e => {
      cases2.push(e.Cases);
    });
    data3.map(e => {
      cases3.push(e.Cases);
    });

    ctx = document.getElementById("myChart").getContext("2d");
    ctx2 = document.getElementById("myChart2").getContext("2d");
    createGraph(dates, cases, cases2, cases3);
  });
  const createGraph = (dates, cases, cases2, cases3) => {
    var myChart = new Chart(ctx, {
      type: "bar",
      data: {
        labels: [...dates],
        datasets: [
          {
            label: "confirmed",
            data: [...cases],
            backgroundColor: "black",
            borderColor: "black",
            borderWidth: 1
          }
        ]
      },
      options: {
        scales: {
          legend: {
            labels: {
              fontColor: "black",
              defaultFontSize: 8
            }
          },
          xAxes: [
            {
              ticks: {
                fontSize: 8
              },
              gridLines: {
                display: false
              }
            }
          ],
          yAxes: [
            {
              ticks: {
                fontSize: 8
              }
            }
          ]
        }
      }
    });
    var chart2 = new Chart(ctx2, {
      type: "line",

      data: {
        labels: [...dates],
        datasets: [
          {
            label: "Deaths",
            backgroundColor: "rgba(255,0,0,0)",
            borderColor: "black",
            borderWidth: 1,
            pointRadius: 1,

            data: [...cases2]
          },
          {
            label: "Recovered",
            borderWidth: 1,
            backgroundColor: "rgba(255,0,0,0)",
            borderColor: "grey",
            pointRadius: 1,
            data: [...cases3]
          }
        ]
      },

      options: {
        scales: {
          xAxes: [
            {
              gridLines: {
                display: false
              }
            }
          ],
          xAxes: [
            {
              ticks: {
                fontSize: 8
              },
              gridLines: {
                display: false
              }
            }
          ],
          yAxes: [
            {
              ticks: {
                fontSize: 8
              }
            }
          ]
        }
      }
    });
  };
</script>

<style>
  .card {
    margin-top: -50px;
    box-shadow: 0 5px 10px rgba(0, 0, 0, 0.12);
    padding: 20px;
    border-radius: 8px;
    margin-left: 10px;
    transform: scale(0.8);
  }
  .graphs {
    margin-left: -75px;
  }
  .flex {
    display: flex;
    justify-content: center;
  }
  .sub-txt {
    font-size: 0.9em !important;
    font-weight: 500;
    margin-bottom: 10px;
  }
  .loading {
    margin-top: 20px;
    height: 0;
    width: 0;
    padding: 10px;
    border: 6px solid #ccc;
    border-right-color: #888;
    border-radius: 22px;
    text-align: center;
    margin-left: -30px;
    -webkit-animation: rotate 1s infinite linear;
  }
  @-webkit-keyframes rotate {
    100% {
      -webkit-transform: rotate(360deg);
    }
  }
  @media (max-width: 640px) {
    .graphs {
      margin-left: -6px;
    }
    .card {
      transform: scale(1);
      width: 280px;
      padding: 0px;
      margin-top: 0px;
      margin-bottom: 15px;
    }
  }
</style>

<main>
  {#if res && res2 && res3}
    <div class="graphs">
      <center>
        <p class="sub-txt">Confirmed Cases in {country}</p>
      </center>
      <div class="card">
        <canvas id="myChart" class="g1" />
      </div>
      <center>
        <p class="sub-txt">Deaths vs Recovered in {country}</p>
      </center>

      <div class="card">
        <canvas id="myChart2" class="g1" />
      </div>
    </div>
  {:else}
    <div class="flex">
      <div class="loading" />
    </div>
  {/if}

</main>
