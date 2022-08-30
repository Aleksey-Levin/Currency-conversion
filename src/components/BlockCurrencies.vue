<template>
  <div class="blockCurrencies">
    <Input :headerValue="textHeader"
           :currentCurrency="this.currentCurrency"
           class="Input"
           @updateValue="onUpdateValue"
           :readonly="this.disabled"
           :value-inp="this.valueInp"
           :type="this.type"
    />
    <b-dropdown id="dropdown-dropright" dropright text="*Тык*" variant="primary" class="m-2">
      <div class="check" style="max-height: 100px; overflow-y: scroll">
        <b-dropdown-item href="#" v-for="(value, name) in currencyList" :key="value"
                         @click="updateCurrentCurrency(name)">{{ name }}
        </b-dropdown-item>
      </div>
    </b-dropdown>
  </div>
</template>

<script>

import Input from "@/components/Input";

export default {
  name: "BlockCurrencies",
  components: {Input},
  props: {
    textHeader: {
      type: String,
      required: false
    },
    currencyList: {
      type: Object,
      required: true
    },
    valueInp: {
      type: String,
      required: false
    },
    disabled: {
      type: Boolean,
      required: false
    },
    type: {
      type: String,
      required: true
    }
  },

  data() {
    return {
      currentCurrency: '',
      value: ''
    }
  },
  methods: {
    updateCurrentCurrency(current) {
      this.currentCurrency = current;
      this.$emit('updateCurrentCurrencies', {
        where: this.textHeader,
        currentCurrencies: this.currentCurrency,
      })
    },
    onUpdateValue(data) {
      this.value = data.value;
      this.$emit('updateValue', {
        value: data.value
      })
    }
  },
}
</script>

<style scoped lang="scss">
.edit-btn {
  cursor: pointer;
}

#dropdown-dropright {
  height: 3em;
}
</style>