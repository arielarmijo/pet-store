<script>

export default {

  props: {
    product: Object
  },

  data() {
    return {
      qty: '1'
    }
  },

  emits: ['productCardEvent'],

  methods: {
    limitStr(string, limit) {
      return string.length < limit ? string : string.slice(0, limit) + '...';
    },
    formatPrice(number) {
      const options = {
        style: 'currency',
        currency: 'CLP'
      }
      const formatter = new Intl.NumberFormat('es', options);
      return formatter.format(number);
    },
    emitProduct(event) {
      this.$emit('productCardEvent', {qty: parseInt(this.qty), product: this.product})
    }
  }

}

</script>

<template>
  <div>
    <b-card
      :title="limitStr(product.name, 30)"
      title-tag="h5"
      :img-src="product.photo"
      img-alt="Image"
      img-top
      tag="div"
      style="max-width: 10rem;"
      class="m-2">
      <b-card-text>Precio: $ {{formatPrice(product.price)}}</b-card-text>
      <div class="buttons-container">
        <b-form-input type="number" v-model="qty"></b-form-input>
        <b-button href="#" variant="primary" class="ms-2" @click="emitProduct">+</b-button>
      </div>
      
    </b-card>
  </div>
</template>

<style>
.buttons-container {
  display: flex;
}
</style>