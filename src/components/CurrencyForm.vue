<template>
  <div class="row">
    <div class="col-5">
      <div class="form-group currency-form__form-group">
        <input
          type="number"
          class="form-control currency-form__input"
          v-model="leftSelected.value"
          @change="calcRightValue"
        />
        <select
          class="form-control currency-form__currency"
          v-model="leftSelected.name"
          @change="calcRightValue"
        >
          <option
            v-for="currencyName of Object.keys(currencyRates)"
            :key="currencyName"
          >
            {{ currencyName }}
          </option>
        </select>
      </div>
    </div>
    <div class="col-2 currency-form__exchange">
      <img src="@/assets/swap.png" width="31px" height="20px" />
    </div>
    <div class="col-5">
      <div class="form-group currency-form__form-group">
        <input
          type="number"
          class="form-control currency-form__input"
          v-model="rightSelected.value"
          @change="calcLeftValue"
        />
        <select
          class="form-control currency-form__currency"
          v-model="rightSelected.name"
          @change="calcRightValue"
        >
          <option
            v-for="currencyName of Object.keys(currencyRates)"
            :key="currencyName"
          >
            {{ currencyName }}
          </option>
        </select>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    currencyRates: {
      required: true,
    },
  },
  data() {
    return {
      leftSelected: {
        value: 1,
        name: "USD",
      },
      rightSelected: {
        value: 1,
        name: "EUR",
      },
    };
  },
  mounted() {
    this.calcRightValue();
  },
  computed: {
    leftCurrencyRate() {
      return this.currencyRates[this.leftSelected.name];
    },
    rightCurrencyRate() {
      return this.currencyRates[this.rightSelected.name];
    },
  },
  methods: {
    calcLeftValue() {
      const { rightSelected, rightCurrencyRate, leftCurrencyRate } = this;
      this.leftSelected.value =
        rightSelected.value * (leftCurrencyRate / rightCurrencyRate);
    },
    calcRightValue() {
      const { leftSelected, leftCurrencyRate, rightCurrencyRate } = this;
      this.rightSelected.value =
        leftSelected.value * (rightCurrencyRate / leftCurrencyRate);
    },
  },
};
</script>

<style>
.currency-form__exchange {
  display: flex;
  justify-content: center;
  align-items: center;
}
.currency-form__form-group {
  display: flex;
  justify-content: space-between;
  margin-bottom: 0;
}
.currency-form__input {
  width: 65%;
  flex-grow: 0;
}
.currency-form__currency {
  flex-grow: 0;
  flex-basis: 32%;
}
</style>
