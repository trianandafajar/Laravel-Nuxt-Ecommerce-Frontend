<template>
  <div class="container-fluid mt-custom">
    <div id="mycarousel" class="carousel slide" data-bs-ride="carousel" data-bs-interval="4000">
      <div class="carousel-inner">
        <!-- Iterating over sliders array -->
        <div class="carousel-item" v-for="(slider, id) in sliders" :class="{ active: id === 0 }" :key="slider.id">
          <a :href="slider.link" target="_blank">
            <img :src="slider.image" class="d-block w-100 rounded" :alt="'Slider ' + id" />
          </a>
        </div>
      </div>
      
      <!-- Carousel Controls -->
      <button class="carousel-control-prev" type="button" data-bs-target="#mycarousel" data-bs-slide="prev">
        <span class="carousel-control-prev-icon" aria-hidden="true"></span>
        <span class="visually-hidden">Previous</span>
      </button>
      <button class="carousel-control-next" type="button" data-bs-target="#mycarousel" data-bs-slide="next">
        <span class="carousel-control-next-icon" aria-hidden="true"></span>
        <span class="visually-hidden">Next</span>
      </button>
    </div>
  </div>
</template>

<script>
export default {
  async fetch() {
    // Fetching sliders data from the API
    await this.$store.dispatch('web/slider/getSlidersData');
  },
  computed: {
    // Accessing sliders data from Vuex store
    sliders() {
      return this.$store.state.web.slider.sliders;
    }
  }
}
</script>

<style scoped>
/* Optional: Add custom styles for the carousel if needed */
</style>
