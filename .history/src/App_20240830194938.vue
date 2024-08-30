<template>
  <div id="app">
    <!-- Include the Header component -->
    <HeaderComponent @currencyChanged="updateMainCurrency" />

    <!-- Route outlet -->
    <router-view :key="$route.fullPath" :mainCurrency="mainCurrency" />
  </div>
</template>

<script>
import HeaderComponent from './components/HeaderComponent.vue'

export default {
  components: {
    HeaderComponent
  },
  data() {
    return {
      mainCurrency: 'USD' // Default main currency
    }
  },
  methods: {
    updateMainCurrency(currency) {
      this.mainCurrency = currency
    }
  }
}
</script>

<style>
/* Add global styles here if necessary */
</style>
