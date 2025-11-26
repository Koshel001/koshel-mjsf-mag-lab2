<script setup>
import { defineProps, defineEmits } from "vue";

const props = defineProps({
  label: { type: String, required: true },
  color: { type: String, default: "blue" },   // options: blue, red, green, gray
  size: { type: String, default: "md" },      // options: sm, md, lg
  icon: { type: String, default: null }       // text icon (for example, "🔍")
});

// Events
const emit = defineEmits(["click"]);

// Sizes
const sizeClasses = {
  sm: "px-2 py-1 text-sm",
  md: "px-4 py-2 text-base",
  lg: "px-6 py-3 text-lg",
};

// Colors
const colorClasses = {
  blue: "bg-blue-500 hover:bg-blue-600 text-white",
  red: "bg-red-500 hover:bg-red-600 text-white",
  green: "bg-green-500 hover:bg-green-600 text-white",
  gray: "bg-gray-300 hover:bg-gray-400 text-black",
};
</script>

<template>
  <button
      :class="[
      'flex items-center gap-2 rounded-lg font-medium transition focus:outline-none focus:ring-2 focus:ring-offset-2',
      sizeClasses[size],
      colorClasses[color]
    ]"
      @click="emit('click')"
  >
    <!-- Icon (if has) -->
    <span v-if="icon">{{ icon }}</span>
    <span>{{ label }}</span>
  </button>
</template>


<style scoped>

</style>