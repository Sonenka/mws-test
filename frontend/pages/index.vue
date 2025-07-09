<template>
  <div class="wrapper">
    <div class="card">
      <FormComponent />
      <img class="corner-cut" src="../public/Corner.svg" width="88" height="88"></img>
      <img class="line" src="../public/AccentLine.svg" width="80" height="104"></img>
    </div>
  </div>
</template>

<script setup lang="ts">
import FormComponent from '@/components/FormComponent.vue'
</script>

<style scoped>
.wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background-color: #F2F3F7;
}

.card {
  background-color: white;
  display: flex;
  flex-direction: column;
  width: 1552px;
  padding: 32px 32px 64px;
  border-radius: 32px;
  position: relative;
}

.card::before {
  content: '';
  position: absolute;
  top: 0;
  right: 0;
  width: 88px;
  height: 88px;
  background-color: #F2F3F7;
  z-index: 1;
}

.corner-cut {
  position: absolute;
  top: 0;
  right: 0;
  z-index: 2;
}

.line {
  position: absolute;
  bottom: -2px;
  left: -2px;
  z-index: 2;
}
</style>