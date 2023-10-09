<script>
export default {
  data() {
    return {
      todoId: 1,
      todoData: null,
      stockData : []
    }
  },
  methods: {
    async fetchData() {
      this.todoData = null
      const res = await fetch(
        `https://jsonplaceholder.typicode.com/todos/${this.todoId}`
      )
      this.todoData = await res.json()

      //twelve api
      this.stockData = null
      const url = 'https://twelve-data1.p.rapidapi.com/stocks?country=United%20States&exchange=NASDAQ&symbol=AAPL';//&format=json
      
      // real time price
      const rUrl = 'https://twelve-data1.p.rapidapi.com/price?symbol=AMZN&format=json&outputsize=30';
      
      // average
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
        const response = await fetch(aUrl, options);
        const result = await response.json();//.text();
        this.stockData = result;
        console.log(result);
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
  <p>Todo id: {{ todoId }}</p>
  <button @click="todoId++">Fetch next todo</button>
  <p v-if="!todoData">Loading...</p>
  <pre v-else>{{ todoData }}</pre>

  <label for="name">Ticker:</label>
  <input type="text" id="name" name="name" required placeholder="AAPL"/>
  <button @click="todoId++">find</button>
  
  <!-- <ul v-for="data in stockData">  
    <li v-for="d in data">
      {{ d }}
    </li>
  </ul> -->
  <!-- <p>{{ stockData }}</p> -->
  <ul>  
    <li v-for="(stock, index) in stockData" :key="index">
      {{ stock.symbol }} ' avgprice: 
    </li>
  </ul>

  <pre>{{ stockData }}</pre>

</template>