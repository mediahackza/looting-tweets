<script>
  let api = 'http://localhost:8888/api/looting-api'
  let tweets = []
  let days = []
  import chroma from 'chroma-js'
  let scale = chroma
    .scale([
      '#ffffff',
      '#ffffd9',
      '#edf8b1',
      '#c7e9b4',
      '#7fcdbb',
      '#41b6c4',
      '#1d91c0',
      '#225ea8',
      '#253494',
      '#081d58',
    ])
    .domain([0, 500])

  async function getTweets() {
    let url = `${api}/tweets.php`
    // console.log(url)

    await fetch(url)
      .then((response) => response.json())
      .then((data) => {
        data.forEach((d) => {
          d.hour = +d.hour
        })
        tweets = data
        days = [...new Set(data.map((tweet) => tweet.date))]
        console.log(tweets)
      })
  }
  getTweets()
  //   let days = [
  //     '2021-07-09',
  //     '2021-07-10',
  //     '2021-07-11',
  //     '2021-07-12',
  //     '2021-07-13',
  //     '2021-07-14',
  //     '2021-07-15',
  //     '2021-07-16',
  //     '2021-07-17',
  //     '2021-07-18',
  //     '2021-07-19',
  //   ]
  let hours = [
    1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21,
    22, 23, 24,
  ]

  function getScale(date, hour) {
    let scaleVal = '#ffffff'

    let num = tweets.filter((d) => d.hour === hour && d.date === date)
    console.log(num)

    if (num.length > 0) {
      scaleVal = scale(num[0].count)
    }

    return scaleVal
  }
  getScale()

  let promise = Promise.all([getTweets()])
</script>

<main>
  <h3>#KZNShutDown</h3>
  {#if promise}
    <div class="table">
      <div class="row">
        <div class="hours">
          <div class="block day" />
          <div class="block hour label">1</div>
          <div class="block hour label">2</div>
          <div class="block hour label">3</div>
          <div class="block hour label">4</div>
          <div class="block hour label">5</div>
          <div class="block hour label">6</div>
          <div class="block hour label">7</div>
          <div class="block hour label">8</div>
          <div class="block hour label">9</div>
          <div class="block hour label">10</div>
          <div class="block hour label">11</div>
          <div class="block hour label">12</div>
          <div class="block hour label">13</div>
          <div class="block hour label">14</div>
          <div class="block hour label">15</div>
          <div class="block hour label">16</div>
          <div class="block hour label">17</div>
          <div class="block hour label">18</div>
          <div class="block hour label">19</div>
          <div class="block hour label">20</div>
          <div class="block hour label">21</div>
          <div class="block hour label">22</div>
          <div class="block hour label">23</div>
        </div>
      </div>
      {#each days as day}
        <div class="row">
          <div class="block day">{day}</div>
          <div class="hours">
            {#each hours as hour}
              <div
                class="block hour"
                style="background: {getScale(day, hour)}; "
              />
            {/each}
          </div>
        </div>
      {/each}
    </div>
  {/if}
</main>

<style>
  main {
    font-size: 0.8rem;
    width: 90%;
    max-width: 900px;
    margin-left: auto;
    margin-right: auto;
  }

  .row {
    display: grid;
    grid-template-columns: 3fr repeat(12, 1fr);
    width: 100%;
    /* border: solid 1px black; */
  }
  .block,
  .hour {
    border: solid 1px #fff;
    background: #eee;
    width: 25px;
    height: 25px;
  }

  .hour {
    /* width: 10px;
    height: 10px; */
  }
  .hours {
    /* border: solid 1px red; */
    width: 100%;
    display: grid;
    grid-template-columns: repeat(24, 1fr);
  }
  .day {
    border: none;
    background: none;
    text-align: right;
    width: 100px;
    padding-right: 10px;
  }
  .label {
    text-align: center;
    font-size: 0.6rem;
    background: none;
    padding-top: 0px;
    padding-bottom: 0px;
    transform: translate(0px, 5px);
    border: solid 1px rgba(255, 255, 255, 0);
  }
</style>
