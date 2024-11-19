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
            @click="nextTab"
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
            'px-4 py-2 rounded-md text-sm font-medium',
            selectedTab === tab.id
              ? 'bg-white shadow text-gray-900'
              : 'bg-gray-200 text-gray-500 hover:bg-gray-300',
          ]"
        >
          {{ tab.title }}
        </button>
      </div>
    </div>

    <!-- Контент активного таба -->
    <div class="mt-6 p-4">
      <component :is="currentTabContent"></component>
    </div>
  </div>
</template>

<script>
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
        { id: 'description', title: 'Описание вакансии' },
        { id: 'search', title: 'Поиск кандидатов' },
        { id: 'publications', title: 'Публикации (19)' },
        { id: 'team', title: 'Команда вакансии' },
        { id: 'funnel', title: 'Воронка найма' },
      ],
      selectedTab: 'description', // По умолчанию первый таб
    }
  },
  computed: {
    currentTabContent() {
      // Используем switch-case для повышения читаемости
      switch (this.selectedTab) {
        case 'description':
          return 'TabDescription'
        case 'search':
          return 'TabSearch'
        case 'publications':
          return 'TabPublications'
        case 'team':
          return 'TabTeam'
        case 'funnel':
          return 'TabFunnel'
        default:
          return 'TabDescription' // Дефолтное значение
      }
    },
    isLastTab() {
      return this.selectedTab === this.tabs[this.tabs.length - 1].id
    },
  },
  methods: {
    selectTab(tabId) {
      this.selectedTab = tabId
    },
    nextTab() {
      const currentIndex = this.tabs.findIndex((tab) => tab.id === this.selectedTab)
      if (currentIndex < this.tabs.length - 1) {
        this.selectedTab = this.tabs[currentIndex + 1].id
      }
    },
    saveDraft() {
      console.log('Черновик сохранен!')
    },
  },
}
</script>
