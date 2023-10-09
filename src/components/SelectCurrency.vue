<template>
     <div>
          <select @change="changeSelectedCurrency($event.target.value)"  class="input">
               <option v-for="currency in currencyArray" :key="currency.ID" :value="currency.CharCode"
                    :selected="selectedCurrency?.CharCode == currency.CharCode">
                    {{ currency.CharCode }} ({{ currency.Name }})
               </option>
          </select>
     </div>
</template>
   
<script lang="ts">
import { defineComponent } from 'vue';
import type { Currency } from '@/interfaces'
export default defineComponent({
     name: 'SelectCurrency',
     props: {
          currencyArray: { type: Object, required: true },
          selectedCurrency: { type: Object, required: false }
     },
     emits: ['currentCurrency'],
     data() {
          return {
               localSelectedCurrency: this.selectedCurrency as Currency
          }
     },
     methods: {
          changeSelectedCurrency(currency: string): void {
               this.localSelectedCurrency = Object.values(this.currencyArray).find(e => e.CharCode == currency)
               localStorage.setItem('selectedCurrency', JSON.stringify(this.localSelectedCurrency))
          }
     },
     watch: {
          localSelectedCurrency():void {
               this.$emit('currentCurrency', this.localSelectedCurrency)
          }
     }
})
</script>