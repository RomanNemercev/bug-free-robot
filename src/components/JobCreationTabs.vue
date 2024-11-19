<template>
  <div>
    <div class="bg-gray-100 p-4 rounded-md">
      <!-- Заголовок и кнопки -->
      <div class="flex items-center justify-between border-b border-gray-300 pb-2">
        <h1 class="text-xl font-semibold">Новая вакансия</h1>
        <div class="flex space-x-2">
          <button
            class="bg-gray-200 px-4 py-2 rounded-md text-gray-600 hover:bg-gray-300"
            @click="saveDraft"
          >
            В черновик
          </button>
          <button
            class="bg-blue-500 px-4 py-2 rounded-md text-white hover:bg-blue-600"
            @click="handleNextTab"
            :disabled="isLastTab"
          >
            Сохранить и продолжить
          </button>
        </div>
      </div>

      <!-- Табы -->
      <div class="flex mt-4 space-x-4">
        <button
          v-for="tab in tabs"
          :key="tab.id"
          @click="selectTab(tab.id)"
          :class="[
            selectedTab === tab.id
              ? 'bg-white shadow text-gray-900'
              : 'bg-gray-200 text-gray-500 hover:bg-gray-300',
          ]"
          class="px-4 py-2 rounded-md text-sm font-medium"
        >
          {{ tab.title }}
        </button>
      </div>
    </div>

    <!-- Контент активного таба -->
    <div class="mt-6 p-4">
      <!-- Динамическая отрисовка контента -->
      <component
        :is="currentTab.component"
        :data="tabData[selectedTab]"
        @validate="validateTab"
        @update-data="updateTabData"
        ref="activeTab"
      ></component>
    </div>
  </div>
</template>

<script>
// Импортируем все табы
import TabDescription from './pages/new-vacancy/TabDescription.vue'
import TabSearch from './pages/new-vacancy/TabSearch.vue'
import TabPublications from './pages/new-vacancy/TabPublications.vue'
import TabTeam from './pages/new-vacancy/TabTeam.vue'
import TabFunnel from './pages/new-vacancy/TabFunnel.vue'

export default {
  name: 'JobCreationTabs',
  components: {
    TabDescription,
    TabSearch,
    TabPublications,
    TabTeam,
    TabFunnel,
  },
  data() {
    return {
      tabs: [
        { id: 'description', title: 'Описание вакансии', component: 'TabDescription' },
        { id: 'search', title: 'Поиск кандидатов', component: 'TabSearch' },
        { id: 'publications', title: 'Публикации (19)', component: 'TabPublications' },
        { id: 'team', title: 'Команда вакансии', component: 'TabTeam' },
        { id: 'funnel', title: 'Воронка найма', component: 'TabFunnel' },
      ],
      selectedTab: 'description', // По умолчанию первый таб
      tabData: {
        description: {}, // Данные для таба Описание
        search: {}, // Данные для таба Поиск
        publications: {}, // Данные для таба Публикации
        team: {}, // Данные для таба Команда
        funnel: {}, // Данные для таба Воронка
      },
      isTabValid: true, // Флаг для проверки валидности текущего таба
    }
  },
  computed: {
    currentTab() {
      return this.tabs.find((tab) => tab.id === this.selectedTab)
    },
    isLastTab() {
      return this.selectedTab === this.tabs[this.tabs.length - 1].id
    },
  },
  methods: {
    selectTab(tabId) {
      this.selectedTab = tabId
    },
    async handleNextTab() {
      const isValid = await this.validateTab()
      if (!isValid) {
        console.log('Ошибка валидации. Переход невозможен.')
        return
      }

      const currentIndex = this.tabs.findIndex((tab) => tab.id === this.selectedTab)
      if (currentIndex < this.tabs.length - 1) {
        this.selectedTab = this.tabs[currentIndex + 1].id
      }
    },
    saveDraft() {
      console.log('Черновик сохранен!', this.tabData)
    },
    async validateTab() {
      const currentTab = this.$refs.activeTab // Получаем текущий таб через ref
      if (currentTab && typeof currentTab.validate === 'function') {
        const isValid = await currentTab.validate() // Вызываем валидацию таба
        if (!isValid) {
          console.log('Ошибка валидации на уровне компонента')
          return false
        }
      }

      // Дополнительная проверка всех input с required
      const inputs = document.querySelectorAll('[required]')
      for (const input of inputs) {
        if (!input.value.trim()) {
          input.classList.add('error') // Добавляем класс для отображения ошибки
          console.log(`Ошибка валидации: поле "${input.placeholder || input.name}" пустое.`)
          return false
        }
        input.classList.remove('error') // Убираем ошибку, если поле заполнено
      }

      return true // Все поля валидны
    },
    updateTabData(tabId, data) {
      // Обновляем данные конкретного таба
      this.$set(this.tabData, tabId, data)
    },
  },
}
</script>
