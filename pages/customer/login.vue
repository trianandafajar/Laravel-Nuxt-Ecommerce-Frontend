<template>
  <div class="container mt-custom mb-3">
    <div class="fade-in">
    <div class="row justify-content-center">
      <div class="col-md-4">
        <div class="card-group">
          <div class="card border-top-orange border-0 shadow-sm rounded">
            <div class="card-body">
              <h3>LOGIN</h3>
              <hr>
              <div v-if="validation.message" class="mt-2">
                <b-alert show variant="danger">{{ validation.message }}</b-alert>
              </div>
              <form @submit.prevent="login">
                <div class="input-group mb-3">
                  <div class="input-group-prepend">
                    <span class="input-group-text">
                      <i class="fa fa-envelope"></i>
                    </span>
                  </div>
                  <input class="form-control" v-model="user.email" type="email" placeholder="Email Address">
                </div>
                <div v-if="validation.email" class="mt-2">
                  <b-alert show variant="danger">{{ validation.email[0] }}</b-alert>
                </div>
                <div class="input-group mb-4">
                  <div class="input-group-prepend">
                    <span class="input-group-text">
                      <i class="fa fa-lock"></i>
                    </span>
                  </div>
                  <input class="form-control" v-model="user.password" type="password" placeholder="Password">
                </div>
                <div v-if="validation.password" class="mt-2">
                  <b-alert show variant="danger">{{ validation.password[0] }}</b-alert>
                </div>
                <div class="row">
                  <div class="col-12">
                    <button class="btn btn-warning shadow-sm rounded-sm px-4 w-100" type="submit">LOGIN</button>
                  </div>
                </form>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="text-center mt-3">
      Belum punya akun? <nuxt-link :to="{name: 'customer-register'}" class="font-weight-bold">Daftar Sekarang</nuxt-link>
    </div>
    </div>
  </div>
</template>

<script>
export default {
  // Middleware untuk mengecek autentikasi
  middleware: "authenticated",

  // Layout yang digunakan
  layout: "default",

  // Metadata halaman
  head() {
    return {
      title: "Login - Customer",
    };
  },

  data() {
    return {
      user: {
        email: "",
        password: "",
      },
      validation: {}, // Default sebagai objek kosong untuk menghindari error undefined
    };
  },

  methods: {
    async login() {
      try {
        await this.$auth.loginWith("customer", {
          data: {
            email: this.user.email,
            password: this.user.password,
          },
        });

        // Fetching data keranjang setelah login
        this.$store.dispatch("web/cart/getCartsData");
        this.$store.dispatch("web/cart/getCartPrice");

        // Redirect ke dashboard pelanggan
        this.$router.push({ name: "customer-dashboard" });
      } catch (error) {
        // Pastikan error memiliki response
        this.validation = error.response?.data || { message: "Terjadi kesalahan saat login." };
      }
    },
  },
};
</script>
