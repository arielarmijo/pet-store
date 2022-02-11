<script>
export default {
  
  props: {
    item: Object
  },

  computed: {
    product() {
      return this.item.product;
    },
    quantity() {
      return this.item.qty;
    }
  }
}
</script>

<template>
  <div class="container">
    <img class="col-5" :src="product.photo"/>
    <div class="col">
      <h5>{{product.name}}</h5>
      <p>Código: {{product.code}}</p>
      <p>Precio: ${{product.price}}</p>
      <p>Cantidad: {{quantity}}</p>
      <p>Subtotal: $ {{product.price * quantity}}</p>
    </div>
  </div>
  <div class="description">
    <p>{{product.description}}</p>
  </div>
</template>

<style>
.container {
  display: flex;
}
.description {
  max-height: 120px;
  overflow-y: scroll;
}
</style>