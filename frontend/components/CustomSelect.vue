<template>
  <div 
    class="custom-select"
    :class="{ active: isOpen }"
    @click="toggle"
    @blur="close"
    ref="selectContainer"
    tabindex="0"
  >
    <div class="select-header" :class="{ 'placeholder-active': !selectedLabel }">
      {{ selectedLabel || placeholder }}
      <span class="arrow" :class="{ 'arrow-up': isOpen }">
        <img src="../public/Chevron.svg" alt="arrow" />
      </span>
    </div>
    <div class="select-options" v-if="isOpen">
      <div
        class="select-option"
        v-for="option in options"
        :key="option[optionValue] || option"
        @click.stop="select(option)"
        :class="{ selected: isSelected(option) }"
      >
        {{ option[optionLabel] || option }}
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, watch } from 'vue'

const props = defineProps({
  modelValue: {
    type: [String, Number],
    default: ''
  },
  options: {
    type: Array as () => Array<Record<string, any>>,
    default: () => []
  },
  placeholder: {
    type: String,
    default: 'Выберите вариант'
  },
  optionLabel: {
    type: String,
    default: 'name'
  },
  optionValue: {
    type: String,
    default: 'id'
  }
})

const emit = defineEmits(['update:modelValue'])

const isOpen = ref(false)
const selectedLabel = ref('')
const selectContainer = ref<HTMLElement | null>(null)

const toggle = () => {
  isOpen.value = !isOpen.value
}

const close = () => {
  isOpen.value = false
}

const select = (option: any) => {
  const value = option[props.optionValue] || option
  const label = option[props.optionLabel] || option
  emit('update:modelValue', value)
  selectedLabel.value = label
  isOpen.value = false
}

const isSelected = (option: any) => {
  return (option[props.optionValue] || option) === props.modelValue
}

const resetSelect = () => {
  emit('update:modelValue', '')
  selectedLabel.value = ''
  isOpen.value = false
}

watch(() => props.modelValue, (newVal) => {
  const found = props.options.find(opt => {
    return (opt[props.optionValue] || opt) === newVal
  })
  if (found) {
    selectedLabel.value = found[props.optionLabel] || found
  } else {
    selectedLabel.value = ''
  }
})

defineExpose({
  resetSelect
})
</script>


<style scoped>
.custom-select {
  position: relative;
  width: 100%;
  padding: 6px 12px;
  border: 1px solid #BBC1C7;
  border-radius: 8px;
  cursor: pointer;
  background: white;
  outline: none;
  min-height: 38px;
  box-sizing: border-box;
  font-size: 16px;
  line-height: 24px;
  transition: 0.3s ease;
}

.custom-select.active,
.custom-select:hover {
  border-color: #1d2023;
}

.select-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 8px;
}

.arrow {
  transition: transform 0.2s ease;
  display: flex;
  align-items: center;
  color: #666;
}

.arrow-up {
  transform: rotate(180deg);
}

.select-options {
  position: absolute;
  top: calc(100% + 4px);
  left: 0;
  right: 0;
  z-index: 1000;
  border: 1px solid #ccc;
  border-radius: 4px;
  background: white;
  max-height: 200px;
  overflow-y: auto;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  font-family: 'Roboto', sans-serif;
}

.select-option {
  padding: 8px 12px;
  font-family: 'Roboto', sans-serif;
}

.select-option:hover {
  background-color: #f5f5f5;
}

.select-option.selected {
  background-color: #e9e9e9;
}

.select-header.placeholder-active {
  color: #6D7782;
}
</style>
