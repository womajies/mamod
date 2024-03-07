<template>
  <div class="app">
    <UForm v-if="!isSuccessRegistration" class="form" @submit="submitForm">
      <UPageTitle :level="1" class="container">Регистрация</UPageTitle>

      <fieldset class="form__body container">
        <h2 class="form__body-title">Заполните Ваши данные</h2>

        <div class="form__body-wrapper">
          <UFormInput v-model="form.username" label="Имя" name="username" :error="errors.username"
            @input="errors.username = ''" />

          <UFormInput v-model="form.email" label="Email" name="email" :error="errors.email"
            @input="errors.email = ''" />

          <UFormSelect v-model="form.role" :options="roleList" :error="errors.role" @input="errors.role = ''" />

          <UFormInput v-model="form.password" type="password" label="Пароль" name="password" :error="errors.password"
            @input="errors.password = ''" />
          <UFormInput v-model="form.password_repeat" type="password" label="Повторите пароль" name="password_repeat"
            :error="errors.password_repeat" @input="errors.password_repeat = ''" />
        </div>
      </fieldset>

      <div class="form__footer container">
        <UFormSwitch v-model="form.public" name="public"
          label="Хотите чтобы Ваш профиль видели другие участники платформы?"
          description="Включает профиль для просмотра другими пользователями по ссылке" />

        <div class="form__footer-bottom">
          <UFormCheckbox v-model="form.privacy" name="privacy">
            Регистрируясь, Вы соглашаетесь с <a href="#">политикой конфиденциальности</a> и <a href="#">обработкой
              персональных данных</a>
          </UFormCheckbox>

          <UButton class="form__submit" variant="primary" :loading="isLoading" :disabled="!form.privacy" type="submit">
            Зарегистрироваться
          </UButton>
        </div>

      </div>
    </UForm>
    <div v-else class="success">
      <UPageTitle :level="2" class="container">Регистрация прошла успешно!</UPageTitle>

      <UButton variant="primary" @click="isSuccessRegistration = false">Начать заново</UButton>
    </div>
  </div>
</template>

<script setup>
import UForm from '@/components/ui/form/UForm.vue';
import UPageTitle from '@/components/ui/UPageTitle.vue';
import UFormInput from './components/ui/form/UFormInput.vue';
import UFormSelect from './components/ui/form/UFormSelect.vue';
import UFormSwitch from './components/ui/form/UFormSwitch.vue';
import UFormCheckbox from './components/ui/form/UFormCheckbox.vue';
import UButton from '@/components/ui/UButton.vue';
import { ref, reactive } from 'vue';

const isLoading = ref(false);
const isSuccessRegistration = ref(false);
const data = ref(null);

const form = reactive({
  username: '',
  email: '',
  role: null,
  public: false,
  password: '',
  password_repeat: '',
  privacy: true
});

const errors = reactive({
  username: '',
  email: '',
  role: '',
  password: '',
  password_repeat: ''
});

const roleList = reactive([
  { value: 1, name: 'Frontend Developer' },
  { value: 2, name: 'Backend Developer' },
  { value: 3, name: 'Fullstack Developer' },
]);

async function submitForm() {
  try {
    isLoading.value = true;

    const user = reactive({
      username: form.username,
      email: form.email,
      role: form.role,
      public: form.public,
      password: form.password,
      password_repeat: form.password_repeat
    })

    await fetch('https://jsonplaceholder.typicode.com/posts', {
      method: 'POST',
      body: JSON.stringify(user),
      headers: {
        'Content-type': 'application/json; charset=UTF-8',
      },
    })
      .then((response) => {
        if (response.ok) {
          return response.json()
        }
      })
      .then((json) => {
        let count = 0;

        for (let key in errors) {
          if (!json[key]) {
            errors[key] = 'Обязательное поле!';
            count++;
          }
        }

        if (count === 0) {
          isSuccessRegistration.value = true
        }

        data.value = json
        return data.value;
      });

  } catch (err) {
    console.error(err);
  } finally {
    isLoading.value = false
  }
}

</script>

<style lang="scss" scoped>
.app {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  padding: rem(16);
}

.success {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  overflow: hidden;
  max-width: rem(600);
  min-height: rem(200);
  width: 100%;
  margin: auto;
  text-align: center;
  background-color: $white-color;
  border-radius: $border-radius-form;

  .u-page-title {
    margin-bottom: rem(32);
  }
}

.form {
  overflow: hidden;
  max-width: rem(960);
  width: 100%;
  margin: auto;
  background-color: $white-color;
  border-radius: $border-radius-form;

  @include on-breakpoint('lg') {
    max-width: rem(800);
  }

  .u-page-title {
    padding-top: 17px;
    padding-bottom: 17px;
  }

  &__body {
    margin: 0;
    padding-top: 17px;
    padding-bottom: 30px;
    border: 1px solid $border-color;
    border-left: none;
    border-right: none;

    &-title {
      margin-bottom: rem(34);
      font-size: rem(16);
      line-height: rem(19);
      font-weight: 500;
    }

    &-wrapper {
      display: grid;
      grid-template-columns: 1fr;
      gap: rem(31) rem(14);

      @include on-breakpoint('md') {
        grid-template-columns: 1fr 1fr;
      }
    }

    .select {
      display: flex;

      @include on-breakpoint('md') {
        grid-column-end: span 2;
        margin-left: auto;
        max-width: calc(50% - rem(7)); // 1/2 half gap
      }
    }
  }

  &__footer {
    padding-top: rem(23);
    padding-bottom: rem(65);

    &-bottom {
      display: flex;
      justify-content: space-between;
      align-items: flex-start;
      flex-wrap: wrap;

      .checkbox {
        margin-top: rem(30);
        margin-right: rem(8);
      }
    }
  }

  &__submit {
    margin-top: rem(30);
  }
}
</style>