<template>
  <div class="fetch">
    <label>
      <b-input-group prepend="Ticker" class="mt-3">
        <b-form-input v-model="search"
                      :state="tickerState"
                      trim
                      aria-describedby="input-live-help input-live-feedback"
                      @keyup.enter="fetchData"
        ></b-form-input>
        <!-- This will only be shown if the preceding input has an invalid state -->
        <b-form-invalid-feedback id="input-live-feedback">
          Enter a ticker listed on NYSE / NASDAQ.
        </b-form-invalid-feedback>
      </b-input-group>
    </label>
    <br>
    <br>

    <div class="grid-container">
      <div class="Logo">
        <img class="object3" :src="profile.logo" alt="" width="120"/>
      </div>
      <div class="Details" v-if="this.price && tickerState">
        {{profile["name"] }} <br>
        <hr>
        {{profile["description"] }} <br>
        <hr>
        <div id="pricewithcolor">{{profile["exchange"] + " Price: " + this.price }}</div>
      </div>
    </div>

  </div>
</template>

<script>

export default {
  name: 'FetchComponent',
  computed: {
    tickerState() {
      return !this.error
    }
  },
  data() {
    return {
      search: "",
      price: null,
      error: false,
      profile: "",
      interval: null
    }
  },
  methods: {
    async fetchData() {
      clearInterval(this.interval);

      this.price = null;
      this.fetchPrice();

      this.interval = setInterval(() => {
        this.fetchPrice();
      }, 10000);

      await fetch("https://api.polygon.io/v1/meta/symbols/" + this.search + "/company?&apiKey=" + process.env.VUE_APP_POLYGON_API_KEY)
          .then(response => response.json())
          .then(data => {
            if (data["status"] !== "ERROR")
              this.profile = data;
          });
    },
    fetchPrice() {
      fetch("https://api.twelvedata.com/price?symbol=" + this.search + "&apikey=" + process.env.VUE_APP_TWELVEDATA_API_KEY)
          .then(response => response.json())
          .then(data => {
            if (data["code"] === 429) {
              this.error = true;
              this.price = "loading...";
            } else if (data["code"] === 400)
              this.error = true;
            else if (data["code"] >= 400) {
              this.error = true;
              this.price = "Error calling price data";
            } else {
              this.error = false;
              this.price = '$' + data["price"].slice(0, -3);
            }

            if(this.error){
              clearInterval(this.interval);
            }
          });
    }
  },

}
</script>

<style scoped>

.Logo {
  margin: 0 20px;
}

.Details {
  width: 500px;
  text-align: start;
}

hr {
  background-color: white;
}

.grid-container {
  place-content: center;
  align-items: start;
  display: grid;
  grid-template-columns: auto auto auto auto;
}

</style>
