<template>
  <div>
    <b-navbar toggleable="lg" class="bg-two" type="dark" fixed="top">
      <b-navbar-brand class="custom" @click.prevent="goToHome" href="#" tag="h1"
        >Lapak H8</b-navbar-brand
      >

      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

      <b-collapse
        v-if="$route.name !== 'Login' && $route.name !== 'Register'"
        id="nav-collapse"
        is-nav
      >
        <b-navbar-nav>
          <b-nav-item @click.prevent="goToHome" href="#"> Home</b-nav-item>
        </b-navbar-nav>

        <b-navbar-nav class="ml-auto">
          <b-nav-item @click.prevent="goToCart" href="#">
            <div v-if="countCarts === 0"><b-icon icon="cart4"></b-icon>Cart</div>
            <div v-else>
              <b-icon icon="cart4"></b-icon>Cart<span class="badge badge-notify">{{
                countCarts
              }}</span>
            </div></b-nav-item
          >
          <b-button
            v-if="!$store.getters.isAuth"
            @click.prevent="$router.push('/login')"
            size="sm"
            class="my-2 my-sm-0 mr-2"
            >Login</b-button
          >
          <b-button
            v-if="!$store.getters.isAuth"
            @click.prevent="$router.push('/register')"
            size="sm"
            class="my-2 my-sm-0 mr-2"
            >Register</b-button
          >
          <b-nav-item-dropdown v-if="$store.getters.isAuth" right>
            <template #button-content
              ><b-icon icon="person-circle"></b-icon> {{ $store.getters.fullname }}
            </template>
            <b-dropdown-item href="#">Orders</b-dropdown-item>
            <b-dropdown-item @click.prevent="logout" href="#">Sign Out</b-dropdown-item>
          </b-nav-item-dropdown>
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>
  </div>
</template>

<script>
export default {
  name: 'Navbar',
  computed: {
    countCarts() {
      return this.$store.getters.carts.length;
    },
  },
  methods: {
    goToCart() {
      if (this.$store.getters.isAuth) {
        this.$router.push('/cart');
      } else {
        this.$router.push('/login');
      }
    },
    goToHome() {
      if (this.$route.path !== '/') {
        this.$router.push('/');
      }
    },
    logout() {
      this.$store.dispatch('logout');
      if (this.$route.path !== '/') {
        this.$router.push('/');
      }
    },
  },
};
</script>

<style scoped>
.badge-notify {
  background: red;
  position: relative;
  top: -12px;
  left: -65px;
}
</style>
