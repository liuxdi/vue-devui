<script setup lang="ts">
import { computed } from 'vue'
import { useData } from 'vitepress'
import { useLocaleLinks } from '../composables/nav'
import { useRepo } from '../composables/repo'
import NavLink from './NavLink.vue'
import NavDropdownLink from './NavDropdownLink.vue'
import enNav from '../../config/enNav'

const { theme } = useData()
const localeLinks = useLocaleLinks()
const repo = useRepo()
const show = computed(() => theme.value.nav || repo.value || localeLinks.value)
let translationTheme = computed( () => localStorage.getItem('preferred_lang') === 'zh-CN' ? theme.value.nav : enNav )
</script>

<template>
  <nav v-if="show" class="nav-links">
    <template v-if="translationTheme">
      <!-- <div v-for="item in theme.nav" :key="item.text" class="item"> -->
      <div v-for="item in translationTheme" :key="item.text" class="item">
        <NavDropdownLink v-if="item.items" :item="item" />
        <NavLink v-else :item="item" />
      </div>
    </template>

    <div v-if="localeLinks" class="item">
      <NavDropdownLink :item="localeLinks" />
    </div>

    <div v-if="repo" class="item">
      <NavLink :item="repo" />
    </div>
  </nav>
</template>

<style scoped>
.nav-links {
  padding: 0.75rem 0;
  border-bottom: 1px solid var(--c-divider);
}

@media (min-width: 720px) {
  .nav-links {
    display: flex;
    padding: 3px 0 0;
    align-items: center;
    border-bottom: 0;
  }

  .item + .item {
    padding-left: 24px;
  }
}
</style>
