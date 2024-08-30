<template>
  <header class="header">
    <nav class="navbar">
      <router-link to="/" class="nav-link">Home</router-link>
      <router-link to="/convert" class="nav-link">Conversion</router-link>
      <div class="currency-selector">
        <label for="currency-dropdown" class="label">Main Currency:</label>
        <select
          id="currency-dropdown"
          v-model="selectedCurrency"
          @change="updateCurrency"
          class="currency-dropdown"
        >
          <option v-for="currency in currencies" :key="currency" :value="currency">
            {{ currency.toUpperCase() }}
          </option>
        </select>
      </div>
    </nav>
  </header>
</template>

<script>
export default {
  data() {
    return {
      selectedCurrency: 'usd',
      currencies: ['usd', 'rub', 'eur']
    }
  },
  methods: {
    updateCurrency() {
      this.$emit('update:currency', this.selectedCurrency)
    }
  }
}
</script>

<style scoped>
.header {
  background-color: #333;
  padding: 15px 20px;
  box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
}

.navbar {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.nav-link {
  color: #fff;
  text-decoration: none;
  font-size: 18px;
  margin-right: 20px;
  transition: color 0.3s;
}

.nav-link:hover {
  color: #ffcc00;
}

.currency-selector {
  display: flex;
  align-items: center;
}

.label {
  color: #fff;
  margin-right: 10px;
  font-size: 16px;
}

.currency-dropdown {
  padding: 8px;
  font-size: 16px;
  border: 1px solid #ddd;
  border-radius: 5px;
  background-color: #f9f9f9;
  color: #333;
  cursor: pointer;
  outline: none;
}

.currency-dropdown:focus {
  border-color: #ffcc00;
  box-shadow: 0px 0px 5px rgba(255, 204, 0, 0.8);
}
</style>
