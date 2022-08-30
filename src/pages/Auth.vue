<template>
  <div class="AuthPage">
    <div class="AuthPage_leftPart">
      <div class="container">
        <h3>
          Конвертация валют
        </h3>
        <p>
          Yet another service. В новом дизайне 🎉
        </p>
      </div>
    </div>
    <div class="AuthPage_rightPart">
      <div class="container">
        <div class="Input-container">
          <BlockCurrencies text-header="From" :currency-list=currencyList :type="'From'" :value-inp = "'0'" @updateCurrentCurrencies="onUpdateCurrentCurrencies" @updateValue = "onUpdateValue" :disabled="checkDisabled"/>
          <BlockCurrencies text-header="To" :currency-list=currencyList :type="'To'" :value-inp = "this.total" @updateCurrentCurrencies="onUpdateCurrentCurrencies" :disabled="true"/>
          <h2 v-if="checkDisabled" class="errorText">Выберите валюты</h2>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
import axios from "axios";
import BlockCurrencies from "@/components/BlockCurrencies";

export default {
  name: "Auth",
  data() {
    return {
      currencyList: {},
      currentCurrencyList: {},
      settingsConvert: {
        'From': '',
        'To': ''
      },
      currentCurrency: '',
      total: "0"
    }
  },
  components: {
    BlockCurrencies,
  },
  methods:{
    onUpdateCurrentCurrencies(data){
      this.settingsConvert[data.where] = data.currentCurrencies;
    },
    onUpdateValue(data){
      this.total = (parseFloat(data.value) * parseFloat(this.currentCurrency)).toString();
    },
    requestCurrentCurrency(){
      axios
          .get(`https://api.apilayer.com/currency_data/live?source=${this.settingsConvert.From}&currencies=${this.settingsConvert.To}`, {headers: {'apikey' : 'v1V7y9X0VsXOXU6TWJX7HTZV3plqp0Mo'}})
          .then(response => this.currentCurrency = response.data.quotes[`${this.settingsConvert.From + this.settingsConvert.To}`])
    },
  },
  watch: {
    settingsConvert:{
      handler(){
        if(Boolean(this.settingsConvert["To"]) && Boolean(this.settingsConvert["From"])){
          this.requestCurrentCurrency();
        }
      },
      deep: true
    },
  },

  mounted() {
    axios
        .get('https://api.apilayer.com/currency_data/list', {headers: {'apikey' : 'v1V7y9X0VsXOXU6TWJX7HTZV3plqp0Mo'}})
        .then(response => (this.currencyList = response.data.currencies))
  },
  computed: {
    checkDisabled: function(){
      return !(Boolean(this.settingsConvert["To"]) && Boolean(this.settingsConvert["From"]))
    }
  }
}

</script>

<style scoped lang="scss">
@import '../assets/styles/components/AuthPage';
</style>