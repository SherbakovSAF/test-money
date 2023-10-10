<template>
     <div>
          <input autofocus type="number" placeholder="Введите значение для конвертации" class="input"
               :value="modelValue" @input="(event: Event)=> $emit('update:modelValue', (event.target as HTMLInputElement).value)">
     </div>
</template>
<script lang="ts">
export default {
     name: 'InputCurrency',
     props: {
          modelValue: {type: String, required: true}
     }, 
     emits: ['update:modelValue'],
}
</script>