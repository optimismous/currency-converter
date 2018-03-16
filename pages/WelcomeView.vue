<template>
  <v-layout column justify-center align-center>
    <v-flex xs12 sm8 md6>
      <div class="text-xs-center">
        <img src="/static/v.png" alt="Vuetify.js" class="mb-5" />
      </div>
      <currency-converter></currency-converter>
    </v-flex>
  </v-layout>
</template>

<script>
import CurrencyConverter from '../components/CurrencyConverter';

export default {
  name: 'welcome-view',
  components: { CurrencyConverter }
}
</script>
