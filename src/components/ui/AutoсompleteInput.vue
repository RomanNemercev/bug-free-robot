<template>
  <div class="autocomplete input-result">
    <input
      type="text"
      class="autocomplete-input e-input"
      :class="{ error: showError }"
      :placeholder="placeholder"
      @input="onInput"
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
      <li v-if="filteredSuggestions.length === 0" class="autocomplete-item no-suggestions">
        Нет вариантов
      </li>
    </ul>
  </div>
</template>

<script>
import { ref, computed, watch } from 'vue'

export default {
  name: 'AutocompleteInput',
  props: {
    placeholder: { type: String, default: 'Введите текст' },
    required: { type: Boolean, default: false },
    options: { type: Array, default: () => [] },
    modelValue: { type: String, default: '' },
    errorMessage: { type: String, default: 'Это поле обязательное, введите значение' },
  },
  setup(props, { emit }) {
    const inputValue = ref(props.modelValue)
    const showSuggestions = ref(false)
    const showError = ref(false)

    const filteredSuggestions = computed(() => {
      if (!inputValue.value) return []
      return props.options.filter((option) =>
        option.toLowerCase().includes(inputValue.value.toLowerCase())
      )
    })

    const updateValue = () => {
      emit('update:modelValue', inputValue.value)
      validateInput()
    }

    const validateInput = () => {
      if (props.required && !inputValue.value.trim()) {
        showError.value = true
        return false
      }
      showError.value = false
      return true
    }

    const onInput = () => {
      showSuggestions.value = !!inputValue.value.trim()
      updateValue()
    }

    const onBlur = () => {
      showSuggestions.value = false
      validateInput()
    }

    const selectSuggestion = (suggestion) => {
      inputValue.value = suggestion
      emit('update:modelValue', suggestion) // Поддержка v-model
      showSuggestions.value = false // Закрываем список
    }

    watch(
      () => props.modelValue,
      (newVal) => {
        inputValue.value = newVal
        validateInput()
      }
    )

    return {
      inputValue,
      showSuggestions,
      filteredSuggestions,
      showError,
      onInput,
      onBlur,
      selectSuggestion,
      validateInput,
    }
  },
}
</script>

<style scoped>
.autocomplete {
  position: relative;
}

.autocomplete-input {
  width: 100%;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
  outline: none;
}

.autocomplete-input.error {
  border-color: red;
}

.input__list-error {
  color: red;
  font-size: 12px;
  margin-top: 4px;
}

.autocomplete-list {
  position: absolute;
  top: calc(100% + 4px); /* Убираем полоску */
  left: 0;
  right: 0;
  background: white;
  border: 1px solid #ccc;
  border-radius: 4px;
  margin-top: 0; /* Убираем отступ */
  max-height: 150px;
  overflow-y: auto;
  z-index: 10;
}

.autocomplete-item {
  padding: 8px;
  cursor: pointer;
}

.autocomplete-item:hover {
  background-color: #f0f0f0;
}

.autocomplete-item.no-suggestions {
  color: #999;
  text-align: center;
  padding: 8px;
  cursor: default;
}
</style>
