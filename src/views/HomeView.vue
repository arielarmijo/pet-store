<script>
import CategoryList from '../components/CategoryList.vue';
import ProductCard from '../components/ProductCard.vue';
import Modal from '../components/Modal.vue';
import CartItem from '../components/CartItem.vue'

const API_URL = 'http://sva.talana.com:8000/api/product/?format=json';

export default {

  components: {
    CategoryList,
    ProductCard,
    Modal,
    CartItem
  },

  data() {
    return {
      category: {},
      products: [],
      filteredProducts: [],
      loading: true,
      shoppingCart: [],
      currentIem: null,
      showProductModal: false
    }
  },

  created() {
    this.fetchProducts();
  },

  watch: {
    category(newCategory) {
      this.filteredProducts = this.products.filter(p => p.category.id === this.category.id);
    }
  },

  methods: {
    async fetchProducts() {
      const resp = await fetch(API_URL);
      this.products = await resp.json();
      this.filteredProducts = this.products;
      this.loading = false;
    },
    onCategoryChange(event) {
      this.category = event;
    },
    showProduct(event) {
      this.currentItem = event;
      this.showProductModal = true;
    },
    addItem() {
      this.showProductModal = false;
      const foundItem = this.shoppingCart.find(item => item.product.id === this.currentItem.product.id);
      if (foundItem) {
        foundItem.qty = foundItem.qty + this.currentItem.qty;
      } else {
        this.shoppingCart.push(this.currentItem);
      }
      this.shoppingCart.forEach(item => {
        console.log({product: item.product.name, qty: item.qty});
      })
    }
  }

}
</script>

<template>
  <div class="container">
    <aside>
      <CategoryList @category="onCategoryChange" />
    </aside>
    <main>
      <p v-if="loading">Cargando productos...</p>
      <div v-else>
        <h5>{{category.name}}</h5>
        <div>
          <p v-if="filteredProducts.length === 0">No hay productos en esta categoría.</p>
          <div v-else class="products-container">
            <ProductCard v-for="product of filteredProducts" :product="product" @productCardEvent="showProduct"/>
          </div>
        </div>
      </div>
    </main>

    <Teleport to="body">
      <Modal :show="showProductModal" @close="showProductModal = false" @addItem="addItem">
        <template v-slot:header>Agregar producto al carro de compras</template>
        <template v-slot:body>
          <CartItem :item="currentItem"/>
        </template>
      </Modal>
    </Teleport>
  </div>

</template>

<style>
.container {
  display: flex;
}
.products-container {
  display: flex;
  flex-wrap: wrap;
}
</style>