<template>
  <div class="content">
    <div class="header">
      <p class="heading">Заполните форму</p>
      <p>На основе ваших ответов теста мы сформируем рекомендации для применения инструментов<br>у вас в компании</p>
    </div>

    <form @submit.prevent="handleSubmit">
      <div class="inputs">
        <CustomSelect
          v-model="selectedOptionId"
          :options="options"
          placeholder="Выберите вариант"
          option-label="name"
          option-value="id"
          required
        />
        
        <input 
          v-model="email" 
          type="email" 
          placeholder="Электронная почта" 
          required 
        />
      </div>

      <SubmitButton 
      />
    </form>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
import CustomSelect from './CustomSelect.vue'
import SubmitButton from './SubmitButton.vue'
import type { Option } from '@/types/types'
import { validateEmail } from '@/utils/validateEmail'

const options = ref<Option[]>([])
const email = ref('')
const selectedOptionId = ref<string>('')

onMounted(async () => {
  try {
    const res = await fetch('http://localhost:1337/api/options')
    if (!res.ok) throw new Error('Ошибка загрузки опций')
    options.value = (await res.json()).data.map((item: any) => ({
      id: item.id,
      name: item.title 
    }))
  } catch (e) {
    console.error(e)
  }
})

const handleSubmit = async () => {
  // Валидация 
  if (!validateEmail(email.value)) {
    alert('Введите корректный email')
    return
  }

  if (!selectedOptionId.value) {
    alert('Выберите вариант из списка')
    return
  }

  // Отправка формы
  try {
    const res = await fetch('http://localhost:1337/api/forms', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        data: { content: {
          email: email.value,
          optionId: Number(selectedOptionId.value)
        }}
      })
    })

    if (!res.ok) throw new Error('Ошибка отправки')
    
    email.value = ''
    selectedOptionId.value = ''
    alert('Форма отправлена успешно!')
    
  } catch (e) {
    console.error(e)
    alert('Ошибка отправки формы')
  }
}
</script>

<style scoped>
.content {
  display: grid;
  grid-template-columns: 1fr 1fr;
  width: 1488px;
  padding: 32px 229px 0px;
  gap: 12px;
}

.header {
  display: flex;
  flex-direction: column;
  gap: 12px;
  width: 100%;
  padding-right: 20px;
}

.heading {
  font-weight: 500;
  color: #1D2023;
  font-size: 24px;
  line-height: 28px;
}

p {
  font-size: 20px;
  line-height: 28px;
}

form {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.inputs {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

input {
  width: 100%;
  padding: 6px 12px;
  border: #BBC1C7 solid 1px;
  border-radius: 8px;
  cursor: pointer;
  height: 44px;
  transition: 0.3s ease;
  font-family: 'Roboto', sans-serif;
}

input::placeholder {
    font-size: 16px;
    line-height: 24px;
    color: #6D7782;
}

input.active,
input:hover {
  border-color: #1d2023;
}
</style>