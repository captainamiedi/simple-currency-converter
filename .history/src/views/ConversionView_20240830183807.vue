<template>
  <div>
    <h1>Currency Conversion</h1>
    <form>
      <div>
        <select v-model="fromCurrency">
          <option v-for="currency in currencies" :key="currency" :value="currency">
            {{ currency.toUpperCase() }}
          </option>
        </select>
        <input type="number" v-model.number="fromAmount" @input="convert" />
      </div>
      <div>
        <select v-model="toCurrency">
          <option v-for="currency in currencies" :key="currency" :value="currency">
            {{ currency.toUpperCase() }}
          </option>
        </select>
        <input type="number" :value="toAmount" readonly />
      </div>
    </form>
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
      currencies: ['usd', 'rub', 'eur', 'brl', 'kzt', 'idr'],
      fromCurrency: 'usd',
      toCurrency: 'rub',
      fromAmount: 0,
      toAmount: 0,
      rates: {}
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
        this.convert() // Convert on load
      } catch (error) {
        console.error('Error fetching rates:', error)
      }
    },
    convert() {
      const rateKey = `${this.fromCurrency}-${this.toCurrency}`
      const rate = this.rates[rateKey]
      if (rate) {
        this.toAmount = (this.fromAmount * rate).toFixed(2)
      } else {
        this.toAmount = 0
      }
    }
  },
  watch: {
    fromCurrency() {
      this.convert()
    },
    toCurrency() {
      this.convert()
    }
  }
}
</script>
