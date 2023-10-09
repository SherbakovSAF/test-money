<template>
     <div>
          <input autofocus type="number" placeholder="Введите значение для конвертации" class="input"
               :value="modelValue" @input="$emit('update:modelValue', $event.target.value)">
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