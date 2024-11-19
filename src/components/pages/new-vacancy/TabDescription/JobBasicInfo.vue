<template>
  <div class="job-basic-info">
    <h2 class="section-title">Основная информация</h2>
    <div class="grid grid-cols-2 gap-4">
      <div>
        <label class="label">Название должности</label>
        <form @submit.prevent="onSubmit">
          <AutocompleteInput
            v-model="jobTitle"
            placeholder="Название должности"
            :options="jobTitles"
            required
            :showError="jobTitleError"
            :errorMessage="'Название должности обязательно'"
          />
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import AutocompleteInput from '../../../ui/AutoсompleteInput.vue'
// import { ref } from 'vue'

export default {
  components: { AutocompleteInput },
  data() {
    return {
      jobTitle: '',
      jobTitleError: false,
      jobTitles: [
        'Менеджер по продажам',
        'Менеджер по работе с клиентами',
        'Менеджер по закупкам',
        'Аккаунт менеджер',
        'HR Менеджер',
        'Менеджер первичной документации',
      ],
    }
  },
  methods: {
    onSubmit() {
      this.jobTitleError = this.jobTitle.trim() === '' //Простая валидация
      if (!this.jobTitleError) {
        // отправить данные
        console.log('Данные отправлены')
      }
    },
  },
}
</script>
