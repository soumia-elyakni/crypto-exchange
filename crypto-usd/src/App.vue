<template>
  <div class="w-100 mb-3 p-2 d-flex flex-row justify-content-center">
    <h1 class="text-center text-white  d-flex float-center w-50">Crypto Prices</h1>
  </div>
  <CryptoCurrencyTable />
</template>

<script>
import CryptoCurrencyTable from "./components/cryptoCurrencyTable.vue";

  export default {
    name: "App",
    components: {
    CryptoCurrencyTable
},
  };
</script>