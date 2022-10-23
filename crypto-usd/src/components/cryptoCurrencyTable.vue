<template >
    <div class="container mt-5 d-flex flex-column border-danger rounded-3">

        <input type="text" class="form-control text-light bg-dark rounded-0 border-0 my-4" placeholder="Search"
            v-model="textSearch" @keyup="searchCoin()" autofocus />

        <table>
            <thead>
                <tr class="text-light rounded-4 border-0 my-4">
                    <th v-for="(title, index) in titles" :key="index">
                        <button @click="sort(`${title}`)">
                            {{ title }}
                        </button>
                        <hr class="w-100">
                        <hr class="w-100">
                    </th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="( coin, index) in filteredCoins" :key="coin.id" class="mb-3">
                    <td class="text-muted">{{ index +1 }}
                        <hr class="w-100">
                    </td>
                    <td>
                        <img :src="coin.image" :alt="coin.name" style="width: 1.2rem" class="me-2" />
                        <span>
                            {{ coin.name }}
                        </span>
                        <span class="ms-2 text-muted text-uppercase">
                            {{ coin.symbol }}
                        </span>
                        <hr class="w-100">
                    </td>
                    <td>{{ coin.current_price.toLocaleString() }}
                        <hr class="w-100">
                    </td>
                    <td :class="[
                      coin.price_change_percentage_24h > 0
                        ?'text-success'
                        :'text-danger',
                    ]">
                        {{ coin.price_change_percentage_24h }}
                        <hr class="w-100 text-light">
                    </td>
                    <td>{{ coin.total_volume.toLocaleString() }}
                        <hr class="w-100">
                    </td>

                </tr>
            </tbody>
        </table>
        <p class="text-light">
        debug: sort={{currentSort}}
        </p>

        <p class="d-flex flex-row justify-content-evenly w-100 m-auto float-end text-white fw-bold pb-3">
            <button @click="prevPage">Previous</button>
            <button @click="nextPage">Next</button>
        </p>
    </div>
</template>

<script>
export default {
    async mounted() {
        const res = await fetch(
            "https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false"
        );
        const data = await res.json();
        this.coins = data;
        this.filteredCoins = data;
    },
    data() {
        return {
            coins: [],
            filteredCoins : this.filteredCoins,
            titles: ["#", "Coin", "Price", "Price Change", "24h Volume"],
            textSearch: "",
        };
    },
    methods: {
        searchCoin() {
            this.filteredCoins = this.coins.filter(
                (coin) =>
                    coin.name.toLowerCase().includes(this.textSearch.toLowerCase()) ||
                    coin.symbol.toLowerCase().includes(this.textSearch.toLowerCase())
            );
        },
        sort:function(s) {
      //if s == current sort, reverse
       if(s === this.currentSort) {
        this.currentSortDir = this.currentSortDir==='asc'?'desc':'asc';
      }
      this.currentSort = s;
    }
        

        // nextPage: function () {
        //     if ((this.currentPage * this.pageSize) < this.coins.length) this.currentPage++;
        // },
        // prevPage: function () {
        //     if (this.currentPage > 1) this.currentPage--;
        // }
    },
    computed : {
        filteredCoin: function() {
     let filter = new RegExp(this.titles, 'i')
     return this.filteredCoins.filter(el => el.title.match(filter))
    }
  }

}





</script>

<style lang="css">
.container {
    margin: auto;
    width: 90%;
    background-color: rgb(42, 41, 41);
    border-radius: 5%;
}

table {
    width: 90%;
    margin: auto;
    border: none;
    color: rgb(230, 234, 238);
}


thead {
    width: 100%;
}

tbody {
    width: 100%;
}

tr {
    text-align: left;
}
</style>