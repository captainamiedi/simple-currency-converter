<template>
  <header>
    <nav>
      <router-link to="/">Home</router-link>
      <router-link to="/convert">Conversion</router-link>
      <select v-model="selectedCurrency" @change="updateCurrency">
        <option v-for="currency in currencies" :key="currency" :value="currency">
          {{ currency }}
        </option>
      </select>
    </nav>
  </header>
</template>

<script>
export default {
  data() {
    return {
      currencies: ['USD', 'EUR', 'RUB'],
      selectedCurrency: 'USD'
    }
  },
  methods: {
    updateCurrency() {
      this.$emit('currencyChanged', this.selectedCurrency)
    }
  }
}
</script>

<style scoped>
nav {
  background: grey;
  position: fixed;
  top: 0;
  z-index: 100;
  padding: 8px;
}
</style>
