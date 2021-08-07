<template>
  <div>
    <ul>
      <li
        v-for="cart in carts.carts"
        :key="cart.id"
      >
        品名 {{ cart.product.title }}
        數量 {{ cart.qty }}
        單價 {{ cart.product.price }}
        <input
          v-model.number="cart.qty"
          type="number"
          min="1"
          @change="updateCart(cart)"
        >
        <button
          type="button"
          @click="removeCart(cart.id)"
        >
          刪除
        </button>
      </li>
    </ul>
    <button
      type="button"
      @click="removeCarts"
    >
      刪除全部
    </button>
    總計 {{ carts.total }}
    折扣價 {{ carts.final_total }}
  </div>
</template>

<script>
export default {
  name: 'Cart',
  data() {
    return {
      carts: [],
    };
  },
  created() {
    this.getCarts();
  },
  methods: {
    getCarts() {
      const api = `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/cart`;
      this.$http.get(api)
        .then((res) => {
          if (res.data.success) {
            this.carts = res.data.data;
          }
        });
    },
    updateCart(cart) {
      const api = `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/cart/${cart.id}`;
      const data = {
        product_id: cart.product_id,
        qty: cart.qty,
      };
      this.$http.put(api, { data })
        .then((res) => {
          if (res.data.success) {
            this.getCarts();
          }
        });
    },
    removeCart(id) {
      const api = `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/cart/${id}`;
      this.$http.delete(api)
        .then((res) => {
          if (res.data.success) {
            this.getCarts();
          }
        });
    },
    removeCarts() {
      const api = `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/carts`;
      this.$http.delete(api)
        .then((res) => {
          if (res.data.success) {
            this.getCarts();
          }
        });
    },
  },
};
</script>
