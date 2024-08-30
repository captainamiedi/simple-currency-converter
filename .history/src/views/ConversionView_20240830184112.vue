<template>
  <div class="conversion">
    <h1>Currency Conversion</h1>
    <form class="conversion-form">
      <div class="conversion-row">
        <select v-model="fromCurrency" class="currency-select">
          <option v-for="currency in currencies" :key="currency" :value="currency">
            {{ currency.toUpperCase() }}
          </option>
        </select>
        <input
          type="number"
          v-model.number="fromAmount"
          @input="convert"
          class="currency-input"
          placeholder="Amount"
        />
      </div>
      <div class="conversion-row">
        <select v-model="toCurrency" class="currency-select">
          <option v-for="currency in currencies" :key="currency" :value="currency">
            {{ currency.toUpperCase() }}
          </option>
        </select>
        <input
          type="number"
          :value="toAmount"
          readonly
          class="currency-input"
          placeholder="Converted Amount"
        />
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

<style scoped>
.conversion {
  padding: 20px;
}

h1 {
  font-size: 24px;
  margin-bottom: 20px;
}

.conversion-form {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.conversion-row {
  display: flex;
  gap: 10px;
  align-items: center;
}

.currency-select {
  padding: 10px;
  font-size: 16px;
  border: 1px solid #ddd;
  border-radius: 5px;
  background-color: #f9f9f9;
}

.currency-input {
  padding: 10px;
  font-size: 16px;
  border: 1px solid #ddd;
  border-radius: 5px;
  width: 100%;
  background-color: #f9f9f9;
}

.currency-input:read-only {
  background-color: #e9e9e9;
}
</style>
