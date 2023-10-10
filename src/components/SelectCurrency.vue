<template>
     <div>
          <select @change="changeSelectedCurrency($event)"  class="input">
               <option :selected="currencyArray.length != 0" value="null" disabled>Не выбрано</option>
               <option v-for="currency in currencyArray" :key="currency.ID" :value="currency.CharCode"
                    :selected="selectedCurrency?.CharCode == currency.CharCode">
                    {{ currency.CharCode }} ({{ currency.Name }})
               </option>
          </select>
     </div>
</template>
   
<script lang="ts">
// Компоненты vue
import { defineComponent } from 'vue';

//Типы TypeScript
import type { Currency } from '@/interfaces'


export default defineComponent({
     name: 'SelectCurrency',
     props: {
          currencyArray: { type: Array as ()=>Currency[], required: true },
          selectedCurrency: { type: Object as ()=> Currency | null, required: false }
     },
     emits: ['selectedCurrency'],
     data() {
          return {
               localSelectedCurrency: this.selectedCurrency as Currency
          }
     },
     methods: {
          changeSelectedCurrency(event: Event): void {
               const target = event.target as HTMLSelectElement;
               const currencyCharCode = target.value;
               if (currencyCharCode) {
                    this.localSelectedCurrency = this.currencyArray.find((e: Currency) => e.CharCode === currencyCharCode) as Currency;
               }
               localStorage.setItem('selectedCurrency', JSON.stringify(this.localSelectedCurrency));
          }
     },
     watch: {
          localSelectedCurrency():void {
               this.$emit('selectedCurrency', this.localSelectedCurrency)
          }
     }
})
</script>