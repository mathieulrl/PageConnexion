<script lang="ts" setup>
import { defineProps, defineEmits, computed } from 'vue'

const props = defineProps({
  id: String,
  label: String,
  type: String,
  placeholder: String,
  errorMessage: String,
  validMessage: String,
  modelValue: String,
})

const emit = defineEmits(['update:modelValue'])

const messageClass = computed(() => {
  return props.errorMessage !== '' ? 'invalid' : 'valid'
})
</script>

<template>
  <div class="flex flex-col">
    <label :for="id">{{ label }}</label>
    <input
      :id="id"
      :type="type"
      :placeholder="placeholder"
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
      @blur="$emit('blur', $event)"
    />
    <p :class="messageClass">
      {{ errorMessage }}
      {{ validMessage }}
    </p>
  </div>
</template>


<style scoped>
.invalid {
  color: red;
}
.flex {
  display: flex;
}

.flex-col {
  flex-direction: column;
}
</style>