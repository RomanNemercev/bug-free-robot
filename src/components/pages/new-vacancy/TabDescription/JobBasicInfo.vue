<template>
  <div class="job-basic-info">
    <h2 class="section-title">Основная информация</h2>
    <div class="grid grid-cols-2 gap-4">
      <div>
        <label class="label">Название должности</label>
        <AutocompleteInput
          v-model="jobTitle"
          :options="['Программист', 'Дизайнер', 'Менеджер']"
          required
          ref="autocomplete"
        />
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
    }
  },
  methods: {
    async validate() {
      // Вызываем валидацию для инпута
      return this.$refs.autocomplete.validateInput()
    },
  },
}
</script>
