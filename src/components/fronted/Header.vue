<template>
  <header class="l-header">
    <div class="l-header__main">
      <router-link to="/" class="l-header__logo">
        <!-- Able-Sisters Logo -->
        <img src="@/assets/images/able-sisters.png" alt="Able-Sisters">
      </router-link>
      <div
        class="l-header__nav"
        :class="{'is-active': openMenuToggle}"
      >
        <ul class="l-header__list">
          <li class="l-header__item">
            <router-link
              to="/products"
              class="l-header__link"
            >
              <div class="l-header__icon">
                <img src="@/assets/images/icon-product.svg">
              </div>
              <span>手工服飾</span>
            </router-link>
          </li>
          <li class="l-header__item">
            <router-link
              to="/about"
              class="l-header__link"
            >
              <div class="l-header__icon">
                <img src="@/assets/images/icon-star.svg">
              </div>
              <span>關於我們</span>
            </router-link>
          </li>
          <li class="l-header__item">
            <router-link
              to="/orders"
              class="l-header__link"
            >
              <div class="l-header__icon">
                <img src="@/assets/images/icon-cart.svg">
                <span
                  class="l-header__quantity"
                  v-show="carts.length !== 0"
                >
                  {{ carts.length }}
                </span>
              </div>
              <span>購物車</span>
            </router-link>
          </li>
          <li class="l-header__item">
            <router-link
              to="/login"
              class="l-header__link"
            >
              <div class="l-header__icon">
                <img src="@/assets/images/icon-admin.svg">
              </div>
              <span>登入後台</span>
            </router-link>
          </li>
        </ul>
      </div>
      <div
        class="l-header__burger"
        :class="{'is-active': openMenuToggle}"
        @click="openMenuToggle = !openMenuToggle"
      >
        <span></span>
        <span></span>
      </div>
    </div>
  </header>
</template>

<script>
export default {
  data() {
    return {
      carts: [],
      openMenuToggle: false,
    };
  },
  created() {
    this.getCart();
    this.$bus.$on('get-cart', () => {
      this.getCart();
    });
  },
  methods: {
    getCart() {
      const api = `${process.env.VUE_APP_APIPATH}/${process.env.VUE_APP_UUID}/ec/shopping`;

      this.$http.get(api).then((res) => {
        this.carts = res.data.data;
      });
    },
  },
};
</script>

<style>

</style>
