<template>
  <div>
    <h1>Exchange Rates</h1>
    <div v-if="rates">
      <div v-for="currency in currencies" :key="currency">
        <p>
          1 {{ mainCurrency.toUpperCase() }} =
          {{ rates[`${mainCurrency.toLowerCase()}-${currency.toLowerCase()}`] }}
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
