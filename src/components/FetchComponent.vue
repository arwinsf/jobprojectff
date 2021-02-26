<template>
  <div class="fetch">
    <label>
      <b-input-group prepend="Ticker" class="mt-3">
        <b-form-input v-model="search"
                      :state="tickerState"
                      trim
                      aria-describedby="input-live-help input-live-feedback"
                      @keyup.enter="fetchData(search)"
        ></b-form-input>
        <b-form-invalid-feedback id="input-live-feedback">
          {{ this.errorMsg }}
        </b-form-invalid-feedback>
      </b-input-group>
    </label>
    <div v-if="this.price && tickerState">
      <hr>
      <h3>
        ({{ profile["symbol"] }}) {{ profile["name"] }}
      </h3>
      <br>
      <div class="grid-container">
        <div class="Logo" style="margin: 0 20px">
          <img class="object3" :src="profile.logo" alt="" width="120"/>
        </div>
        <div class="Details" style="width: 550px; text-align: start">
          <h2 style="float:left; white-space:nowrap;overflow:hidden;text-overflow:ellipsis;"
              id="pricewithcolordollars">
            ${{ this.price.split(".")[0] }}
          </h2>
          <h5 style="padding-top: 1px" id="pricewithcolorcents">
            <span :style="priceColor">{{ this.price.split(".")[1] }}</span>
          </h5>
          <hr style="margin-top: 25px">
          {{ profile["description"] }}
        </div>
      </div>
      <hr>
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
      priceColor: null,
      error: false,
      errorMsg: null,
      profile: "",
      interval: null
    }
  },
  methods: {
    async fetchData(search) {
      this.price = null;
      await fetch("https://api.polygon.io/v1/meta/symbols/" + search.toUpperCase() +
          "/company?&apiKey=" + process.env.VUE_APP_POLYGON_API_KEY)
          .then(response => response.json())
          .then(data => {
            if (data["status"] !== "ERROR")
              this.profile = data;
          });
      await fetch("https://api.twelvedata.com/price?symbol=" + search.toUpperCase() +
          "&apikey=" + process.env.VUE_APP_TWELVEDATA_API_KEY)
          .then(response => response.json())
          .then(data => {
            if (data["code"] === 429) {
              this.error = true;
              this.errorMsg = "Too many calls at once..."
              this.price = "loading...";
            } else if (data["code"] === 400) {
              this.error = true;
              this.errorMsg = "Enter a ticker listed on NYSE / NASDAQ.";
            } else if (data["code"] >= 400) {
              this.error = true;
              this.errorMsg = "Error.";
            } else {
              this.error = false;
              this.price = data["price"].slice(0, -3);
            }
          });
    }
  }
}
</script>

<style scoped>
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
