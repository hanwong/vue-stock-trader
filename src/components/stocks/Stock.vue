<template>
  <div class="col-md-4 col-sm-6">
    <div class="panel panel-success">
      <div class="panel-heading">
        <h3 class="panel-title">
          {{ propStock.name }}
          <small>(Price: {{ propStock.price }} )</small>
        </h3>
      </div>
      <div class="panel-body">
        <div class="pull-left">
          <input 
              type="number"
              class="form-control"
              placeholder="Quantity"
              v-model.number="quantity"
              :class="{danger: insufficientFunds}"
          >
        </div>
        <div class="pull-right">
          <button
              class="btn btn-success"
              @click="buyStock"
              :disabled="insufficientFunds || quantity <= 0 || !Number.isInteger(quantity)"
              >{{ insufficientFunds ? 'Too Much' : 'Buy'}}
              </button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scpoed>
  .danger,
  .danger:focus {
    border: 1px solid red;
  }
</style>

<script>
  export default {
    props: ['propStock'],
    
    data() {
      return {
        quantity: 0
      }
    },
    computed: {
      funds() {
        return this.$store.getters.funds;
      },
      insufficientFunds() {
        return this.quantity * this.propStock.price > this.funds;
      }
    },
    methods: {
      buyStock() {
        const order = {
          stockId: this.propStock.id,
          stockPrice: this.propStock.price,
          quantity: this.quantity,
        };
        this.$store.dispatch('buyStock', order);
        this.quantity = 0;
      }
    }

  }
</script>