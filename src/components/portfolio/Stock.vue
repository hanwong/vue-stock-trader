<template>
  <div class="col-md-4 col-sm-6">
    <div class="panel panel-info">
      <div class="panel-heading">
        <h3 class="panel-title">
          {{ propStock.name }}
          <small>(Price: {{ propStock.price }} | Quantity: {{propStock.quantity }})</small>
        </h3>
      </div>
      <div class="panel-body">
        <div class="pull-left">
          <input 
              type="number"
              class="form-control"
              placeholder="Quantity"
              v-model.number="quantity"
              :class="{danger: insufficientQuantity}"
          >
        </div>
        <div class="pull-right">
          <button
              class="btn btn-success"
              @click="sellStock"
              :disabled="insufficientQuantity || quantity <= 0 || !Number.isInteger(quantity)"
              >{{insufficientQuantity ? 'Too Much' : 'Sell'}}
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
  import { mapActions } from 'vuex';

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
      insufficientQuantity() {
        return this.quantity > this.propStock.quantity;
      }
    },
    methods: {
      ...mapActions({
        placeSellOrder: 'sellStock'
      }),
      sellStock() {
        const order = {
          stockId: this.propStock.id,
          stockPrice: this.propStock.price,
          quantity: this.quantity
        };
        this.placeSellOrder(order);
        this.quantity = 0;
      }
    }

  }
</script>