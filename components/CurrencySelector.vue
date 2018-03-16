<template>
    <div>
        <v-select
          v-model="selected"
          :items="currencies"
          label="From"
          single-line
        ></v-select>
    </div>
</template>

<script>
import { VSelect } from 'vuetify';

export default {
  name: "CurrencySelector",
  props: ['selected'],
  components: {
      VSelect
  },
  data() {
    return {
      currencies: [
        { text: "BTC" },
        { text: "LTC" },
        { text: "USD" },
        { text: "RUB" }
      ]
    };
  }
};
</script>
