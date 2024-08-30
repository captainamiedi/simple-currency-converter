<template>
  <div id="app">
    <!-- Include the Header component -->
    <Header @currencyChanged="updateMainCurrency" />

    <!-- Route outlet -->
    <router-view :key="$route.fullPath" :mainCurrency="mainCurrency" />
  </div>
</template>

<script>
import Header from './components/Header.vue'

export default {
  components: {
    Header
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
