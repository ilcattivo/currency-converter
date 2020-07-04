<template>
  <div class="conventer">
    <h1 class="conventer__title">Конвентер валют</h1>
    <div class="container">
      <CurrencyForm v-if="currencyRates" :currencyRates="currencyRates" />
    </div>
  </div>
</template>

<script>
import CurrencyForm from "./components/CurrencyForm";

export default {
  name: "App",
  components: {
    CurrencyForm,
  },
  data() {
    return {
      currencyRates: null,
    };
  },
  async mounted() {
    const res = await fetch("https://api.exchangeratesapi.io/latest?base=USD");
    const { rates } = await res.json();
    // transforming object into array
    this.currencyRates = rates;
  },
};
</script>

<style>
.conventer {
  width: 700px;
  margin: 200px auto;
}
.conventer__title {
  text-align: center;
  margin-bottom: 20px;
  font-size: 1.8rem;
}
</style>
