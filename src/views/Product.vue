<template>
  <div>
    <h1>{{ product.title }}</h1>
  </div>
</template>

<script>
export default {
  name: 'Product',
  data() {
    return {
      product: {},
    };
  },
  created() {
    if (this.$route.params.id) {
      const { id } = this.$route.params;
      this.getProduct(id);
    }
  },
  methods: {
    getProduct(id) {
      const api = `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/product/${id}`;
      this.$http.get(api)
        .then((res) => {
          if (res.data.success) {
            this.product = res.data.product;
          } else {
            this.$router.push('/products');
          }
        });
    },
  },
};
</script>
