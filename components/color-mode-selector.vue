<template>
  <div class="flex space-x-2 items-center">
    <p class="text-gray-500 text-xl md:text-xs" v-if="showNextModelLabel">
      Change to {{ nextMode }}
    </p>
    <button
      @click="toggleMode()"
      @mouseenter="showNextModelLabel = true"
      @mouseleave="showNextModelLabel = false"
      class="hover:bg-gray-200 dark:hover:bg-gray-600 px-2 py-1 text-gray-500 text-3xl md:text-base"
    >
      {{ nextModeIcon }}
    </button>
  </div>
</template>

<script setup>
const colorMode = useColorMode();

const showNextModelLabel = ref(false);

const modes = ["system", "light", "dark"];
const nextModeIcons = {
  system: "🌓",
  light: "🌕",
  dark: "🌑",
};

const nextMode = computed(() => {
  let currentModeIndex = modes.indexOf(colorMode.preference);
  if (currentModeIndex === 2) currentModeIndex = 0;
  else currentModeIndex += 1;
  return modes[currentModeIndex];
});
const nextModeIcon = computed(() => {
  return nextModeIcons[nextMode.value];
});

const toggleMode = function () {
  colorMode.preference = nextMode.value;
};
</script>
