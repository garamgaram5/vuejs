<script>
export default {
  data() {
    return {
      stockId: 1,
      stockData : null,
      ticker: 'AAPL',
      realTimePrice: 0
    }
  },
  methods: {
    async fetchData() {
      //twelve api
      this.stockData = null
      // this.ticker = ticker
      // NASDAQ stocks
      const url = `https://twelve-data1.p.rapidapi.com/stocks?country=United%20States&exchange=NASDAQ&symbol=${this.ticker}`;//&format=json
      
      // real time price
      const rUrl = `https://twelve-data1.p.rapidapi.com/price?symbol=${this.ticker}`; //&format=json&outputsize=30
      
      // average price
      const aUrl = 'https://twelve-data1.p.rapidapi.com/avgprice?interval=1day&symbol=AAPL&outputsize=30&format=json';

      // price target is available exclusively with enterprise plans. $999/month
      const targetUrl = 'https://twelve-data1.p.rapidapi.com/price_target?symbol=%3CREQUIRED%3E';

      const options = {
        method: 'GET',
        headers: {
          'X-RapidAPI-Key': '02eb38d73bmsh241a5df8dd65348p1789a4jsn379dcfa6b009',
          'X-RapidAPI-Host': 'twelve-data1.p.rapidapi.com'
        }
      };

      try {
        const response = await fetch(url, options);
        const stockData = await response.json();//.text();
        this.stockData = stockData.data;
        console.log(stockData);

        const rResponse = await fetch(rUrl, options);
        const realTimePrice = await rResponse.json();//.text();
        this.realTimePrice = realTimePrice;

      } catch (error) {
        console.error(error);
      }
      
    }
  },
  mounted() {
    this.fetchData()
  },
  watch: {
    todoId() {
      this.fetchData()
    }
  }
}
</script>

<template>
  <h1>Stock search</h1>
  <label for="ticker">Ticker:</label>
  <input v-model="ticker" placeholder="AAPL or AMZN"/> {{ ticker }}
  <button @click="fetchData">find</button>
  <p v-if="!stockData">Loading...</p>
  <!-- <pre v-else>{{ stockData }}</pre> -->
  <!-- <pre v-else>{{ realTimePrice }}</pre> -->

  <ul>  
    <li v-for="(stock, index) in stockData" :key="index">
      {{ stock.symbol }} : {{ stock.name }}, realTimePrice: {{ realTimePrice.price }}
    </li>
  </ul>

</template>