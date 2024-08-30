<template>
  <div class="home">
    <h1>Exchange Rates</h1>
    <div v-if="rates" class="rate-list">
      <div v-for="currency in currencies" :key="currency" class="rate-item">
        <p>
          1 {{ mainCurrency.toUpperCase() }} =
          <span class="rate">{{
            rates[`${mainCurrency.toLowerCase()}-${currency.toLowerCase()}`]
          }}</span>
          {{ currency.toUpperCase() }}
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  props: {
    mainCurrency: {
      type: String,
      default: 'usd'
    }
  },
  data() {
    return {
      rates: null,
      currencies: ['rub', 'eur', 'brl', 'kzt', 'idr']
    }
  },
  async created() {
    await this.fetchRates()
  },
  methods: {
    async fetchRates() {
      try {
        const response = await axios.get('https://status.neuralgeneration.com/api/currency')
        this.rates = response.data
      } catch (error) {
        console.error('Error fetching rates:', error)
      }
    }
  },
  watch: {
    mainCurrency(newCurrency) {
      this.fetchRates()
    }
  }
}
</script>

<style scoped>
.home {
  padding: 20px;
}

h1 {
  font-size: 24px;
  margin-bottom: 20px;
}

.rate-list {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.rate-item {
  background-color: #f5f5f5;
  padding: 10px 15px;
  border-radius: 5px;
  box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
}

.rate {
  font-weight: bold;
  color: #333;
}
</style>
