<template>
  <div>
    <ul>
      <li
        v-for="product in products"
        :key="product.id"
      >
        <router-link
          :to="{
            name: 'Product',
            params: {
              id: product.id
            }
          }"
        >
          {{ product.title }}
        </router-link>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'Products',
  data() {
    return {
      products: [],
      pagination: {},
    };
  },
  created() {
    this.getProducts();
  },
  methods: {
    getProducts(page = 1) {
      const api = `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/products?page=${page}`;
      this.$http.get(api)
        .then((res) => {
          if (res.data.success) {
            this.products = res.data.products;
            this.pagination = res.data.pagination;
          }
        });
    },
  },
};
</script>
