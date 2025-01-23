<template>
  <q-page class="flex flex-center bg-dark text-white">
    <div class="q-pa-md text-center">
      <q-icon name="home" size="100px" color="white" />
      <div class="text-h4 q-mt-md">Добро пожаловать на наш сайт</div>
      <div class="text-subtitle1 q-mt-sm">
        Исследуйте наш сайт и узнайте больше о наших услугах и продуктах.
      </div>

      <q-btn
        label="Начать"
        color="positive"
        class="q-mt-md"
        rounded
        size="lg"
      />

      <div class="q-mt-xl text-h6">Наши преимущества</div>
      <q-separator class="q-my-md" color="white" />

      <div class="row q-gutter-md justify-center">
        <q-card class="my-card bg-positive text-white" bordered>
          <q-card-section>
            <q-icon name="check_circle" size="50px" color="white" />
            <div class="text-h6 q-mt-md">Высокое качество</div>
            <div class="text-subtitle2 q-mt-sm">
              Мы предлагаем только лучшие решения для вас.
            </div>
          </q-card-section>
        </q-card>

        <q-card class="my-card bg-positive text-white" bordered>
          <q-card-section>
            <q-icon name="support" size="50px" color="white" />
            <div class="text-h6 q-mt-md">Поддержка 24/7</div>
            <div class="text-subtitle2 q-mt-sm">
              Мы всегда готовы помочь вам в любое время.
            </div>
          </q-card-section>
        </q-card>

        <q-card class="my-card bg-positive text-white" bordered>
          <q-card-section>
            <q-icon name="trending_up" size="50px" color="white" />
            <div class="text-h6 q-mt-md">Быстрое развитие</div>
            <div class="text-subtitle2 q-mt-sm">
              Наши технологии постоянно улучшаются.
            </div>
          </q-card-section>
        </q-card>
      </div>
    </div>
  </q-page>
</template>

<script setup lang="ts">

</script>

<style scoped>
.my-card {
  width: 300px;
  padding: 20px;
  border-radius: 12px;
  text-align: center;
}
</style>
