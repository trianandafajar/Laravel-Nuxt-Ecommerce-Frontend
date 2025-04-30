<template>
  <div class="card border-0 rounded shadow-sm border-top-orange">
    <div class="card-body">
      <h5>MAIN MENU</h5>
      <hr>
      <ul class="list-group">
        <!-- Dashboard Link -->
        <nuxt-link :to="{name: 'customer-dashboard'}"
          class="list-group-item text-decoration-none text-dark text-uppercase">
          <i class="fa fa-tachometer-alt"></i> Dashboard
        </nuxt-link>

        <!-- My Orders Link -->
        <nuxt-link :to="{name: 'customer-invoices'}" 
          class="list-group-item text-decoration-none text-dark text-uppercase">
          <i class="fa fa-shopping-cart"></i> My Orders
        </nuxt-link>

        <!-- Logout Link -->
        <a @click="logout" class="list-group-item text-decoration-none text-dark text-uppercase"
          style="cursor: pointer;">
          <i class="fa fa-sign-out-alt"></i> Logout
        </a>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    // Logout method to handle user logout
    async logout() {
      try {
        // Logout from authentication
        await this.$auth.logout();

        // Clear cart data from the store
        this.$store.commit('web/cart/SET_CARTS_DATA', []);
        this.$store.commit('web/cart/SET_CART_PRICE', 0);

        // Redirect user to the login page
        this.$router.push({ name: 'customer-login' });
      } catch (error) {
        console.error("Logout failed:", error);
        // Optional: Handle error notification or redirection on logout failure
      }
    }
  }
}
</script>

<style scoped>
/* Style for active menu items */
a.nuxt-link-active {
  background: rgba(255, 222, 212, .05) !important;
}

/* Optional: Add some padding or margin for a cleaner look */
.list-group-item {
  padding: 1rem 1.25rem;
}
</style>
