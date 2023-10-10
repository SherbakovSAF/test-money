
<template>
  <div class="container">
      <h1 class="window-title">Ковертер валют</h1>
      <LoaderUi v-if="isLoader"/>
      <div class="content-wrap" v-else>
          <div >
            <InputCurrency :hidden="!selectedCurrency" v-model="sumToCalc"/>
          </div>
          <SelectCurrency 
            :selectedCurrency="selectedCurrency"
            :currencyArray="currencyArray" @selectedCurrency="selectedCurrency = $event" />
          <p class="input result-window">{{ calcToRubles }}₽</p>
        </div>
  </div>
</template>
<script lang="ts">
import { defineComponent } from 'vue';

// Компоненты
import InputCurrency from '@/components/InputCurrency.vue';
import SelectCurrency from '@/components/SelectCurrency.vue'
import LoaderUi from '@/ui/LoaderUi.vue'

// Типы TypeScript
import type {Currency} from './interfaces'

export default defineComponent({
  name: 'App',
  components: { InputCurrency, SelectCurrency, LoaderUi},
  data() {
    return {
      sumToCalc: "" as string,
      selectedCurrency: null as Currency |  null,
      timer: null as ReturnType<typeof setTimeout> | null,
      currencyArray: [] as Currency[],
      isLoader: true as Boolean
    }
  },
  computed: {
    calcToRubles():number{
      return Number(this.sumToCalc) * (this.selectedCurrency?.Value ?? 0)
    },
  },
  methods: {
    delayedStart() {
      if (this.timer) {
        clearTimeout(this.timer);
      }
      this.timer = setTimeout(this.requestApi, 1000);
    },
    async requestApi(){
      this.isLoader = true
      try {
        const request = await fetch('https://www.cbr-xml-daily.ru/daily_json.js');
        const result = await request.json()
        this.currencyArray = Object.values(result.Valute)
      } catch (error) {
        alert(error)
      } finally {
        this.isLoader = false
      }
    },
  },
  watch: {
    sumToCalc(){
      this.delayedStart()
    }
  },
  mounted() {
    this.requestApi()
    const getSelectedCurrency = localStorage.getItem('selectedCurrency')
    this.selectedCurrency = JSON.parse(getSelectedCurrency ?? 'null')
  }
})
</script>
