<template>
  <div class="autocomplete input-result">
    <input
      type="text"
      class="autocomplete-input e-input"
      :class="{ error: showError }"
      :placeholder="placeholder"
      @input="onInput"
      @focus="onFocus"
      @blur="onBlur"
      v-model="inputValue"
    />
    <span class="input__list-error" v-if="showError">{{ errorMessage }}</span>
    <ul class="autocomplete-list" v-if="showSuggestions">
      <li
        v-for="suggestion in filteredSuggestions"
        :key="suggestion"
        class="autocomplete-item"
        @click="selectSuggestion(suggestion)"
      >
        {{ suggestion }}
      </li>
    </ul>
  </div>
</template>

<script>
import { ref, computed, watch } from 'vue'

export default {
  name: 'AutocompleteInput',
  props: {
    placeholder: { type: String, default: 'Text' },
    required: { type: Boolean, default: false },
    options: { type: Array, default: () => [] },
    showError: { type: Boolean, default: false }, // Управление видимостью ошибки
    errorMessage: { type: String, default: 'Это поле обязательное, введите значение' }, // Сообщение об ошибке
  },
  setup(props) {
    const inputValue = ref('')
    const showSuggestions = ref(false)
    const filteredSuggestions = computed(() => {
      if (!inputValue.value) return []
      return props.options.filter((option) =>
        option.toLowerCase().includes(inputValue.value.toLowerCase())
      )
    })

    const onInput = () => {
      showSuggestions.value = inputValue.value.length > 0
    }
    const onFocus = () => {
      showSuggestions.value = true
    }
    const onBlur = () => {
      setTimeout(() => (showSuggestions.value = false), 200)
    }
    const selectSuggestion = (suggestion) => {
      inputValue.value = suggestion
      showSuggestions.value = false
    }

    return {
      inputValue,
      showSuggestions,
      filteredSuggestions,
      onInput,
      onFocus,
      onBlur,
      selectSuggestion,
    }
  },
}
</script>

<style scoped>
/* Ваши стили */
</style>
