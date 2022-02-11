<script>

const API_URL = 'http://sva.talana.com:8000/api/product-category/?format=json';

export default {
  
  data() {
    return {
      categories: null,
      loading: true
    }
  },

  emits: ['category'],

  created() {
    this.fetchCategories();
  },

  methods: {
    async fetchCategories() {
      const resp = await fetch(API_URL);
      this.categories = await resp.json();
      this.loading = false;
    },
    emitCategory(category) {
      this.$emit('category', category);
    }
  }

}

</script>

<template>
  <p v-if="loading">Cargando...</p>
  <ul v-else>
    <li v-for="category of categories" @click="emitCategory(category)">
      <span>{{category.name}}</span>
    </li>
  </ul>
</template>

<style>
li {
  cursor: pointer;
}
li:hover {
  color: blue;
  transform: translate(5px);
}
</style>