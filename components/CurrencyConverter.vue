<template>
    <div>
        <div class="header">
            <h1>{{ header }}</h1>
        </div>
        <v-flex class="selectors">
          <select class="select" v-model="currencyFrom">
            <option
              v-for="option in currencies"
              v-bind:key="option.text"
              v-bind:value="option.text">
                {{ option.text }}
            </option>
          </select>
          <select class="select" v-model="currencyTo">
            <option
              v-for="option in currencies"
              v-bind:key="option.text"
              v-bind:value="option.text">
                {{ option.text }}
            </option>
          </select>
        </v-flex>
        <v-text-field
          name="fromCurrency"
          label="Введите сумму"
          v-model="from"
        ></v-text-field>
        <div class="content">
            {{result}}
        </div>
        <div class="chart">
          {{chart}}
        </div>
    </div>
</template>

<script>
import CurrencySelector from './CurrencySelector';
import fetch from 'isomorphic-fetch';

export default {
  name: 'currency-converter',
  data() {
    return {
      currencyFrom: 'BTC',
      currencyTo: 'USD',
      from: 1,
      chart: '',
      result: undefined,
      header: 'Currency converter App',
      chartData: [["Jan", 4], ["Feb", 2], ["Mar", 10], ["Apr", 5], ["May", 3]],
      currencies: [
        { text: "BTC" },
        { text: "LTC" },
        { text: "USD" },
        { text: "RUB" }
      ]
    };
  },
  // computed: {
  //   result: function () {
  //     return this.from * 2
  //   }
  // },
  mounted: function() {
    if (window) {
      this.chart = '...chart...';
    }
  },
  created: function() {
    var self = this;
    this.getConversionResult();
  },
  watch: {
    currencyFrom: 'getConversionResult',
    currencyTo: 'getConversionResult',
    from: 'getConversionResult'
  },
  filters: {
    numbersOnly: function (value) {
      if (!value) return '';
      value = value.toString();
      return value.replace(/[^0-9]*/ig, '');
    }
  },
  methods: {
    getConversionResult: function() {
      var self = this;
      fetch(`https://min-api.cryptocompare.com/data/price?fsym=${this.currencyFrom}&tsyms=${this.currencyTo}`)
        .then(function (response) {
          // handle errors
          return response.json();
        })
        .then(function (data) {
          if (data && data[self.currencyTo]) {
            self.result = data[self.currencyTo] * Number(self.from);
          }
        })
        .catch((err) => console.log(err))
    }
  }
};
</script>

<style scoped>
h1,
h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.selectors {
  margin: 0 auto;
  max-width: 300px;
}

.select {
  appearance: listbox;
  background-color: #303030;
  border: 1px solid white;
}

.currency-from {
  float: left;
}

.currency-to {
  float: right;
}
</style>