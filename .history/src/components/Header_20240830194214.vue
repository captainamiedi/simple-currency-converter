<template>
  <header>
    <nav>
      <div>
        <router-link to="/">Home</router-link>
        <router-link to="/convert">Conversion</router-link>
      </div>
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
  display: flex;
  justify-content: space-between;
  padding: 8px;
}
</style>
