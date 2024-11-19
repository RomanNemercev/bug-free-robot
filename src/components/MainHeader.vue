<script>
import { useRoute, useRouter } from 'vue-router'

export default {
  setup() {
    const route = useRoute()
    const router = useRouter()

    const isRouteActive = (path) => {
      return route.path === path
    }

    return { isRouteActive, route, router }
  },
}
</script>

<template>
  <header class="header flex items-center bg-outer-space py-[12.5px] px-[50px]">
    <div class="header__logo">
      <span class="text-white font-bold text-[25px]">JOBLY</span>
    </div>
    <nav class="header__menu">
      <ul class="list-none flex gap-5 p-0 m-0">
        <li>
          <router-link
            to="/applications"
            class="text-base text-white transition-opacity"
            :class="[isRouteActive('/applications') ? 'opacity-100' : 'opacity-50']"
            >Заявки</router-link
          >
        </li>
        <li>
          <router-link
            to="/"
            class="text-base text-white transition-opacity"
            :class="[isRouteActive('/') ? 'opacity-100' : 'opacity-50']"
            >Вакансии</router-link
          >
        </li>
        <li>
          <router-link
            to="/candidates"
            class="text-base text-white transition-opacity"
            :class="[isRouteActive('/candidates') ? 'opacity-100' : 'opacity-50']"
            >Кандидаты</router-link
          >
        </li>
        <li>
          <router-link
            to="/reports"
            class="text-base text-white transition-opacity"
            :class="[isRouteActive('/reports') ? 'opacity-100' : 'opacity-50']"
            >Отчеты</router-link
          >
        </li>
      </ul>
    </nav>
    <div class="header__icons ml-auto">
      <button class="bg-transparent text-slate-500"><i class="icon icon-stats"></i>#</button>
      <button class="bg-transparent text-slate-500"><i class="icon icon-calendar"></i>#</button>
      <button class="bg-transparent text-slate-500"><i class="icon icon-bell"></i>#</button>
      <button class="header__profile">ДМ</button>
    </div>
  </header>
</template>

<style scoped>
.header__icons button {
  margin-left: 10px;
  cursor: pointer;
  font-size: 20px;
  transition: color 0.3s;
}

.header__icons button:hover {
  color: #fff;
}

.header__profile {
  width: 40px;
  height: 40px;
  background-color: #ffffff30;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: bold;
  color: #fff;
  cursor: pointer;
}
</style>

