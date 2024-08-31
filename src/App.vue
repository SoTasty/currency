<template>
  <div id="app">
    <Header @currency-changed="updateBaseCurrency" :currencies="currencies" :baseCurrency="baseCurrency"/>
    <router-view :baseCurrency="baseCurrency" :currencies="currencies"/>
  </div>
</template>

<script>
import Header from './components/Header.vue';

export default {
  components: {
    Header
  },
  data() {
    return {
      baseCurrency: 'RUB',
      currencies: ['RUB', 'USD', 'EUR']
    };
  },
  methods: {
    updateBaseCurrency(newCurrency) {
      this.baseCurrency = newCurrency;
    }
  }
};
</script>
