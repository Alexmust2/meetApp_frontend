<template>
  <q-page class="row items-center justify-evenly">
    <div class="q-pa-md" style="width: 400px">
      <q-form @submit.prevent="handleSubmit">
        <q-input filled v-model="form.username" label="Username" required />
        <q-input filled v-model="form.email" label="Email" type="email" required />
        <q-input filled v-model="form.password" label="Password" type="password" required />
        <q-btn type="submit" label="Create User" color="primary" class="full-width q-mt-md" />
      </q-form>

      <q-banner v-if="successMessage" class="q-mt-md" color="green-2" text-color="green-9">
        {{ successMessage }}
      </q-banner>

      <q-banner v-if="errorMessage" class="q-mt-md" color="red-2" text-color="red-9">
        {{ errorMessage }}
      </q-banner>
    </div>

    <example-component
      title="Example component"
      active
      :todos="todos"
      :meta="meta"
    ></example-component>

    <div class="q-pa-md" style="width: 400px">
      <h2>Users</h2>
      <ul>
        <li v-for="user in users" :key="user.id">
          {{ user.username }} ({{ user.email }})
        </li>
      </ul>
    </div>
  </q-page>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
import type { Todo, Meta } from 'components/models'
import ExampleComponent from 'components/ExampleComponent.vue'
import axios from 'axios' // Импорт как обычный импорт
import type { AxiosError } from 'axios' // Импорт типа отдельно
import { api } from '../boot/axios' // Убедитесь, что путь корректный

// Определение Типов
interface User {
  id: number
  username: string
  email: string
}

interface CreateUserData {
  createUser: User
}

interface CreateUserInput {
  username: string
  email: string
  password: string
}

interface GraphQLError {
  message: string
  // Добавьте другие поля, если необходимо
}

interface GraphQLResponse<T> {
  data?: T
  errors?: GraphQLError[]
}

interface GetUsersData {
  users: User[]
}

// Состояние Компонента
const todos = ref<Todo[]>([
  { id: 1, content: 'ct1' },
  { id: 2, content: 'ct2' },
  { id: 3, content: 'ct3' },
  { id: 4, content: 'ct4' },
  { id: 5, content: 'ct5' },
])

const meta = ref<Meta>({
  totalCount: 1200,
})

// Состояние формы
const form = ref<CreateUserInput>({
  username: '',
  email: '',
  password: '',
})

// Сообщения об успехе и ошибке
const successMessage = ref<string>('')
const errorMessage = ref<string>('')

// Состояние пользователей
const users = ref<User[]>([])

// GraphQL Запросы
const CREATE_USER_MUTATION = `
  mutation CreateUser($input: CreateUserInput!) {
    createUser(input: $input) {
      email
    }
  }
`

const GET_USERS_QUERY = `
  query Users {
    users {
      username
      email
    }
  }
`

// Функция для отправки мутации создания пользователя
const handleSubmit = async () => {
  successMessage.value = ''
  errorMessage.value = ''

  try {
    const response = await api.post<GraphQLResponse<CreateUserData>>('', {
      query: CREATE_USER_MUTATION,
      variables: {
        input: {
          username: form.value.username,
          email: form.value.email,
          password: form.value.password,
        },
      },
    })

    const responseData = response.data

    if (responseData.errors && responseData.errors.length > 0) {
      // Используем определенный тип GraphQLError
      errorMessage.value = responseData.errors.map((err: GraphQLError) => err.message).join(', ')
    } else if (responseData.data) {
      successMessage.value = `User ${responseData.data.createUser.username} created successfully!`
      // Очистка формы после успешного создания
      form.value.username = ''
      form.value.email = ''
      form.value.password = ''

      // Обновление списка пользователей после создания нового
      await fetchUsers()
    } else {
      errorMessage.value = 'Unknown error occurred'
    }
  } catch (error: unknown) {
    if (axios.isAxiosError(error)) {
      const axiosError = error as AxiosError<{ errors?: GraphQLError[]; data?: CreateUserData }>
      if (axiosError.response && axiosError.response.data) {
        const responseData = axiosError.response.data
        if (responseData.errors) {
          errorMessage.value = responseData.errors.map(err => err.message).join(', ')
          return
        }
      }
      errorMessage.value = axiosError.message
    } else if (error instanceof Error) {
      errorMessage.value = error.message
    } else {
      errorMessage.value = 'An unknown error occurred'
    }
  }
}

// Функция для получения пользователей
const fetchUsers = async () => {
  try {
    const response = await api.post<GraphQLResponse<GetUsersData>>('', {
      query: GET_USERS_QUERY,
    })

    const responseData = response.data

    if (responseData.errors && responseData.errors.length > 0) {
      errorMessage.value = responseData.errors.map((err: GraphQLError) => err.message).join(', ')
    } else if (responseData.data) {
      users.value = responseData.data.users
    } else {
      errorMessage.value = 'Unknown error occurred while fetching users'
    }
  } catch (error: unknown) {
    if (axios.isAxiosError(error)) {
      const axiosError = error as AxiosError<{ errors?: GraphQLError[]; data?: GetUsersData }>
      if (axiosError.response && axiosError.response.data) {
        const responseData = axiosError.response.data
        if (responseData.errors) {
          errorMessage.value = responseData.errors.map(err => err.message).join(', ')
          return
        }
      }
      errorMessage.value = axiosError.message
    } else if (error instanceof Error) {
      errorMessage.value = error.message
    } else {
      errorMessage.value = 'An unknown error occurred while fetching users'
    }
  }
}

// Вызов функции для получения пользователей при монтировании компонента
onMounted(async () => {
  await fetchUsers()
})
</script>

<style scoped>
/* Добавьте стили при необходимости */
</style>
