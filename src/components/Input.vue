<template>
  <div>
    <div class="headInput">
      <h3> {{getHeadValue() + ": " + this.currentCurrency}} </h3>
    </div>
    <input
          type="text"
          v-model="value"
          placeholder="Введите кол-во валюты"
          :class="[classObject]"
          @keyup="updateValue"
          :readonly="this.readonly"
    >
    <div class="errorText" v-if = "this.checkRegular"> {{getErrorMessage()}}</div>
  </div>
</template>


<script>
import {errorMessage, headValue} from '@/store/enums/input-data-enums';
export default {
  name: "Input",
  props: {
    headerValue:{
      type: String,
      required: false
    },
    currentCurrency: {
      type: String,
      required: false
    },
    readonly: {
      type: Boolean,
      required: false
    },
    valueInp:{
      type: String,
      required: false
    },
    type:{
      type: String,
      required: true
    }
  },
  data() {
    return {
      value: this.valueInp,
    }
  },
  computed:{
    classObject: function(){
      return {
        errorInput: !/(#^[0-9]+$#)/.test(this.value),
      }
    },
    checkRegular: function(){
      return !/^[1-9]\d*(\.\d+)?$/g.test(this.value);
    },
  },
  methods: {
    updateValue() {
      this.$emit('updateValue', {
        value: this.value
      })
    },
    getErrorMessage(){
      return errorMessage[this.type.toUpperCase()];
    },
    getHeadValue(){
      return headValue[this.type.toUpperCase()];
    },
  },
  watch: {
    valueInp(){
      if(this.type == "To"){
        this.value = this.valueInp;
      }
    }
  }
}
</script>

<style scoped lang="scss">
@import '../assets/styles/components/Input';
</style>



