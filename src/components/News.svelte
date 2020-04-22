<script>
  import axios from "axios";
  import { onMount } from "svelte";

  export let locationData;
  let news = null;
  let showMore = false;
  let showNews = null;
  onMount(async () => {
    const res = await axios.get(
      `https://api.smartable.ai/coronavirus/news/${locationData.countryCode}`,
      {
        headers: {
          "Subscription-Key": "8018cc99a02b46ec87d78cefad4fb250"
        }
      }
    );
    news = res.data.news;
    // console.log("NEWS", res.data.news);
    showNews = news.slice(4, 7);
  });

  const handleMore = () => {
    if (showMore) {
      showNews = news.slice(4, 7);
    } else showNews = news;

    showMore = !showMore;
  };
</script>

<style>
  .main-cont {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
  }

  .card {
    box-shadow: 0 5px 10px rgba(0, 0, 0, 0.12);
    width: 200px;
    padding: 20px;
    border-radius: 8px;
    margin-right: 10px;
  }

  .news-img {
    width: 200px;
    height: auto;
    border-radius: 3px;
  }

  a {
    color: rgb(36, 34, 34);
    font-weight: 400;
    text-decoration: none;
  }
  a:hover {
    color: rgb(141, 141, 141);
  }
  .item {
    margin-top: 13px;
    font-weight: 500;
    margin-bottom: 110px;
  }
  .item:hover {
    cursor: pointer;
    opacity: 0.5;
  }
  .news-cont {
    margin-top: 20px;
    margin-left: 10px;
  }

  @media (max-width: 640px) {
    .card {
      font-size: 0.8em;
    }
    .mb-c {
      width: 280px;
    }
    .news-img {
      width: 200px;
      height: auto;
      border-radius: 3px;
    }
  }
</style>

<main class="news-cont">
  <h2>News</h2>
  <div class="main-cont">
    {#if news}
      {#each showNews as n, i}
        {#if n.images}
          <div class="card mb-c">
            <div class="flex">
              <div>
                <img class="news-img" src={n.images[0].url} alt={n.title} />
              </div>
              <div>
                <a href={n.webUrl} target="_blank">{n.title}</a>
              </div>
            </div>
          </div>
        {/if}
      {:else}
        <p>loading news</p>
      {/each}
    {/if}

  </div>
  <div class="item">
    {#if showMore}
      <p on:click={handleMore}>Less</p>
    {:else}
      <p on:click={handleMore}>More</p>
    {/if}
  </div>

</main>
