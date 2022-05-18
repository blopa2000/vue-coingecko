<template>
  <div class="container">
    <div class="row">
      <h1 class="text-white">Coin Market</h1>

      <input
        type="text"
        class="form-control bg-dark text-light rounded-0 border-0 my-4"
        placeholder="Search coin"
        @keyup="searchCoin"
        v-model="textSeach"
      />
      <table class="table table-dark">
        <thead>
          <tr>
            <th v-for="(title, index) in titles" :key="index">{{ title }}</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(coin, index) in filteredCoins" :key="index">
            <td class="text-muted">
              {{ index + 1 }}
            </td>
            <td>
              <img
                :src="coin.image"
                :alt="coin.name"
                style="width: 2rem"
                class="me-2"
              />
              <span>
                {{ coin.name }}
              </span>

              <span class="ms-2 text-uppercase text-muted">
                {{ coin.symbol }}
              </span>
            </td>
            <td>
              {{ coin.current_price }}
            </td>
            <td
              :class="
                coin.price_change_percentage_24h >= 0
                  ? 'text-success'
                  : 'text-danger'
              "
            >
              {{
                new Intl.NumberFormat("en-US", {
                  style: "currency",
                  currency: "USD",
                }).format(coin.price_change_percentage_24h)
              }}
              %
            </td>
            <td>
              {{
                new Intl.NumberFormat("en-US", {
                  style: "currency",
                  currency: "USD",
                }).format(coin.total_volume)
              }}
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script >
export default {
  name: "App",
  data() {
    return {
      coins: [],
      filteredCoins: [],
      titles: ["#", "Coin", "Price", "Price Change", "Volume (24h)"],
      textSeach: "",
    };
  },

  async mounted() {
    const res = await fetch(
      "https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false"
    );
    const data = await res.json();
    this.coins = this.filteredCoins = data;
  },

  methods: {
    searchCoin(e) {
      const search = e.target.value.toLowerCase();
      this.filteredCoins = this.coins.filter((coin) => {
        return (
          coin.name.toLowerCase().includes(search) ||
          coin.symbol.toLowerCase().includes(search)
        );
      });
    },
  },
};
</script>

<style>
body {
  background: #33393f !important ;
}
</style>
