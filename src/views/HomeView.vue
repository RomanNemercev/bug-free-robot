<script setup>
import MainHeader from '../components/MainHeader.vue'
import JobCreationTabs from '../components/JobCreationTabs.vue'
</script>

<template>
  <div>
    <main-header />
    <job-creation-tabs @tab-changed="handleTabChanged" />
  </div>
</template>

