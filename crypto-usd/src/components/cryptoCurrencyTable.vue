<template>
    <div>
        <table>
        <thead>
          <tr class="table-header">
            <th v-for="(title, index) in titles" :key="index">
              {{ title }}
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(coin, index) in filteredCoins" :key="coin.id">
            <td class="text-muted">{{ index }}</td>
            <td>
              <img :src="coin.image" :alt="coin.name" style="width: 2rem" class="me-2" />
              <span>
                {{ coin.name }}
              </span>
              <span class="ms-2 text-muted text-uppercase">
                {{ coin.symbol }}
              </span>
            </td>
            <td>{{ coin.current_price.toLocaleString() }}</td>
            <td
              :class="[
                coin.price_change_percentage_24h > 0
                  ? 'text-success'
                  : 'text-danger',
              ]"
            >
              {{ coin.price_change_percentage_24h }}
            </td>
            <td>{{ coin.total_volume.toLocaleString() }}</td>
          </tr>
        </tbody>
      </table>
    </div>
</template>

<script>
export default {
  data() {
    return {
      coins: [],
      filteredCoins: [],
      titles: ["#", "Coin", "Price", "Price Change", "24h Volume"],
      textSearch: "",
    };
  },
  async mounted() {
    const res = await fetch(
      "https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false"
    );
    const data = await res.json();
    this.coins = data;
    this.filteredCoins = data;
  },
}

  
</script>

<style lang="css">
div{
    margin: auto;
    margin-top: 10px;
    padding-top: 10px;
    display: flex;
    width: 90%;
    background-color: rgb(42, 41, 41);
    border-radius: 10px;
}
table{
    width: 90%;
    margin: auto;
    border: none;
    color: rgb(230, 234, 238);
}


thead {
    width: 100%;
}

tbody{
    width: 100%;
}

tr{
    text-align: left;
}



</style>