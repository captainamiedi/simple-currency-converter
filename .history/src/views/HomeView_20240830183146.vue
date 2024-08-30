<template>
  <div>
    <h1>Exchange Rates</h1>
    <div v-if="rates">
      <p>1 USD = {{ rates['USD'] }} {{ mainCurrency }}</p>
      <p>1 EUR = {{ rates['EUR'] }} {{ mainCurrency }}</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  props: {
    mainCurrency: {
      type: String,
      default: 'USD'
    }
  },
  data() {
    return {
      rates: null
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
      this.fetchRates() // Fetch rates again if the main currency changes
    }
  }
}
</script>
