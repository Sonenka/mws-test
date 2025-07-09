<template>
  <button 
    type="submit" 
    class="submit-button"
  >
    <slot>Отправить</slot>
  </button>
</template>

<script setup lang="ts">
</script>

<style scoped>
.submit-button {
  font-family: 'Roboto', sans-serif;
  height: 44px;
  width: 100%;
  border-radius: 8px;
  padding: 6px 8px;
  background-color: #FF0032;
  color: white;
  font-weight: 500;
  font-size: 16px;
  line-height: 24px;
  border: none;
  cursor: pointer;
  transition: all 0.3s ease;
}

.submit-button:hover {
  transform: scale(1.02);
}
</style>