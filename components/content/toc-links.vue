<template>
  <ul>
    <li v-for="link in links" :key="link.id">
      <NuxtLink
        :to="{ path: route.path, hash: `#${link.id}` }"
        :class="{
          'ml-4': level,
          'text-green-600 dark:text-green-400': activeId === link.id,
        }"
      >
        {{ link.text }}
      </NuxtLink>
      <toc-links :links="link.children" :level="level + 1" :active-id="activeId"></toc-links>
    </li>
  </ul>
</template>

<script setup lang="ts">
const route = useRoute();
defineProps({
  links: {
    type: Array,
  },
  level: {
    type: Number,
    default: 0,
  },
  activeId: {
    type: String,
    default: null,
  },
});
</script>
