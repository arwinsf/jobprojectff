<template>
  <div class="title">
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
          Enter a ticker listed on the NYSE exchange.
        </b-form-invalid-feedback>
      </b-input-group>
    </label>
    <br>
    <br>
    <h2>{{ price }}</h2>
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
      error: false
    }
  },
  methods: {
    async fetchData() {
      this.price = null;
      await fetch("https://api.twelvedata.com/price?symbol=" + this.search + "&apikey=be06c8a7f6ce4f62ab48d85ec7a1eb63")
          .then(response => response.json())
          .then(data => {
            if (data["code"] === 429) {
              this.error = false;
              this.price = "Too many calls at once...";
            } else if (data["code"] === 400)
              this.error = true;
            else if (data["code"] >= 400) {
              this.error = false;
              this.price = "Error calling price data";
            } else {
              this.error = false;
              this.price = '$' + data["price"].slice(0, -3);
            }
          })
    }
  },

}
</script>

<style scoped>
button {
  background-color: lightblue;
}
</style>
