<template>
  <div class="job-description">
    <!-- Основная информация -->
    <JobBasicInfo />

    <!-- Подробности о компании -->
    <!-- <CompanyDetails /> -->

    <!-- Подробности вакансии -->
    <!-- <VacancyDetails /> -->

    <!-- Заработная плата -->
    <!-- <SalaryDetails /> -->

    <!-- Место работы -->
    <!-- <JobLocation /> -->

    <!-- Контактная информация -->
    <!-- <ContactInfo /> -->
  </div>
</template>

<script>
import JobBasicInfo from './TabDescription/JobBasicInfo.vue'
// import CompanyDetails from './TabDescription/CompanyDetails.vue'
// import VacancyDetails from './TabDescription/VacancyDetails.vue'
// import SalaryDetails from './TabDescription/SalaryDetails.vue'
// import JobLocation from './TabDescription/JobLocation.vue'
// import ContactInfo from './TabDescription/ContactInfo.vue'

export default {
  name: 'TabDescription',
  components: {
    JobBasicInfo,
    // CompanyDetails,
    // VacancyDetails,
    // SalaryDetails,
    // JobLocation,
    // ContactInfo,
  },
}
</script>

<style scoped>
.job-description {
  display: flex;
  flex-direction: column;
  gap: 2rem;
}
</style>
