<template>
  <div>
    <ul>
      <li
        v-for="product in products"
        :key="product.id"
      >
        <div>
          {{ product.title }}
          <button
            type="button"
            @click="getProduct(product.id)"
          >
            查看更多
          </button>
          <button
            type="button"
            :disabled="product.id === status.loadingId"
            @click="addCart(product.id)"
          >
            加到購物車
          </button>
        </div>
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
      status: {
        loadingId: '',
      },
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
    getProduct(id) {
      this.$router.push({ name: 'Product', params: { id } });
    },
    addCart(id) {
      this.status.loadingId = id;
      const api = `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/cart`;
      const data = {
        product_id: id,
        qty: 1,
      };
      this.$http.post(api, { data })
        .then((res) => {
          if (res.data.success) {
            this.status.loadingId = '';
          }
        });
    },
  },
};
</script>

<style lang="scss">
  button {
    &:disabled {
      opacity: .5;
      pointer-events: none;
    }
  }
</style>
