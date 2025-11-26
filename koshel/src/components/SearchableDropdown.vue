<script setup>
import { ref, computed, defineProps, defineEmits } from 'vue'

const props = defineProps({
  items: { type: Array, required: true },
  multiple: { type: Boolean, default: false },
  placeholder: { type: String, default: 'Choose...' }
})

const emit = defineEmits(['update:modelValue'])

const searchQuery = ref('')
const isOpen = ref(false)
const highlightedIndex = ref(0)
const selected = ref(props.multiple ? [] : null)

// Filtration
const filteredItems = computed(() => {
  if (!searchQuery.value) return props.items
  return props.items.filter(i =>
      i.toLowerCase().includes(searchQuery.value.toLowerCase())
  )
})

// Choosing an element
function selectItem(item) {
  if (props.multiple) {
    if (!selected.value.includes(item)) {
      selected.value.push(item)
    }
  } else {
    selected.value = item
    isOpen.value = false
  }
  emit('update:modelValue', selected.value)
  searchQuery.value = ''
  highlightedIndex.value = 0
}

// Clear
function clearSelection() {
  selected.value = props.multiple ? [] : null
  emit('update:modelValue', selected.value)
}

// Key navigation
function onKeyDown(e) {
  if (!isOpen.value) return
  if (e.key === 'ArrowDown') {
    highlightedIndex.value = (highlightedIndex.value + 1) % filteredItems.value.length
  } else if (e.key === 'ArrowUp') {
    highlightedIndex.value = (highlightedIndex.value - 1 + filteredItems.value.length) % filteredItems.value.length
  } else if (e.key === 'Enter') {
    if (filteredItems.value[highlightedIndex.value]) {
      selectItem(filteredItems.value[highlightedIndex.value])
    }
  } else if (e.key === 'Escape') {
    isOpen.value = false
  }
}

// Closing when clicking outside the component
function onClickOutside(event) {
  if (!event.target.closest('.dropdown-container')) {
    isOpen.value = false
  }
}
document.addEventListener('click', onClickOutside)
</script>

<template>
  <div class="relative w-64 dropdown-container" @keydown="onKeyDown">
    <div class="flex items-center gap-2 border rounded-lg px-2 py-1">
      <input
          type="text"
          v-model="searchQuery"
          :placeholder="selected && !multiple ? selected : placeholder"
          @focus="isOpen = true"
          class="flex-grow outline-none"
      />
      <button v-if="selected && ((multiple && selected.length) || (!multiple && selected))"
              @click="clearSelection"
              class="text-gray-500">✖</button>
    </div>

    <ul
        v-if="isOpen"
        class="absolute left-0 right-0 bg-white border rounded-lg mt-1 max-h-40 overflow-auto z-10"
    >
      <li
          v-for="(item, index) in filteredItems"
          :key="item"
          @click="selectItem(item)"
          :class="[
          'px-3 py-2 cursor-pointer',
          index === highlightedIndex ? 'bg-blue-100' : 'hover:bg-gray-100'
        ]"
      >
        <slot name="option" :item="item">
          {{ item }}
        </slot>
      </li>

      <li v-if="filteredItems.length === 0" class="px-3 py-2 text-gray-500">
        Nothing was found
      </li>
    </ul>
  </div>
</template>

<style scoped>

</style>
