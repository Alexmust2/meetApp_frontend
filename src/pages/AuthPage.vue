<template>
  <q-page class="flex flex-center">
    <q-card class="q-pa-lg shadow-2" style="width: 400px">
      <q-card-section class="text-center">
        <q-icon name="login" size="64px" color="primary" />
        <div class="text-h5 q-mt-md q-mb-md">Авторизация</div>
      </q-card-section>

      <q-card-section>
        <q-form @submit="onSubmit" @reset="onReset" class="q-gutter-md">
          <q-input
            v-model="loginForm.email"
            label="Email"
            type="email"
            outlined
            lazy-rules
            :rules="[(val) => !!val || 'Введите email']"
          />

          <q-input
            v-model="loginForm.password"
            label="Пароль"
            type="password"
            outlined
            lazy-rules
            :rules="[(val) => !!val || 'Введите пароль']"
          />

          <q-btn label="Войти" type="submit" color="primary" class="full-width" />
          <q-btn label="Сбросить" type="reset" color="secondary" flat class="full-width" />
        </q-form>
      </q-card-section>

      <q-card-section class="text-center">
        <q-btn label="Забыли пароль?" flat color="primary" @click="onForgotPassword" />
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script setup lang="ts">
import { ref } from 'vue'


const loginForm = ref({
  email: '',
  password: '',
})

const onSubmit = async () => {
  try {
    console.log('Авторизация:', loginForm.value)
    // Эмуляция запроса к API
    await new Promise((resolve) => setTimeout(resolve, 1000))

    alert('Неверные учетные данные')
  } catch (error) {
    console.error('Ошибка авторизации:', error)
  }
}

const onReset = () => {
  loginForm.value.email = ''
  loginForm.value.password = ''
}

const onForgotPassword = () => {
  alert('Функция восстановления пароля в разработке')
}
</script>

<style scoped>
.q-card {
  max-width: 400px;
}
</style>
