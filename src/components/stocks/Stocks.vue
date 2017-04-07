<template>
  <div>
    <app-stock v-for="stock in stocks" :stock="stock"></app-stock>
  </div>
</template>

<script>
  import Stock from './Stock.vue';

  export default {
    data() {
      return {
        stocks: [
          { id: 1, name: 'BENZ', price: 200},
          { id: 2, name: 'AUDI', price: 150},
          { id: 3, name: 'HONDA', price: 180},
          { id: 4, name: 'LEXUS', price: 195}
        ]
      }
    },

    components: {
      appStock: Stock
    }

  }
</script>