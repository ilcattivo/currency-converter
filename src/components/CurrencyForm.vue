<template>
  <div class="row">
    <div class="col-5">
      <div class="form-group currency-form__form-group">
        <input
          type="number"
          class="form-control currency-form__input"
          v-model.number="leftForm.value"
          @keyup="calcRightValue"
          @change="calcRightValue"
        />
        <select
          class="form-control currency-form__currency"
          v-model="leftForm.name"
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
          v-model.number="rightForm.value"
          @keyup="calcLeftValue"
          @change="calcLeftValue"
        />
        <select
          class="form-control currency-form__currency"
          v-model="rightForm.name"
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
      leftForm: {
        value: 1,
        name: "USD",
      },
      rightForm: {
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
      return this.currencyRates[this.leftForm.name];
    },
    rightCurrencyRate() {
      return this.currencyRates[this.rightForm.name];
    },
  },
  methods: {
    calcLeftValue() {
      const { rightForm, rightCurrencyRate, leftCurrencyRate } = this;

      const leftValue =
        rightForm.value * (leftCurrencyRate / rightCurrencyRate);
      this.leftForm.value = +leftValue.toFixed(2);
    },

    calcRightValue() {
      const { leftForm, leftCurrencyRate, rightCurrencyRate } = this;

      const rightValue =
        leftForm.value * (rightCurrencyRate / leftCurrencyRate);
      this.rightForm.value = +rightValue.toFixed(2);
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
  flex-grow: 1;
  margin-right: 5px;
}
.currency-form__currency {
  width: 80px;
}
</style>
