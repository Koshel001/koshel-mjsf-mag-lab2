<script setup>
import { ref } from "vue"
import Button from "./components/Button.vue"
import SearchableDropdown from "./components/SearchableDropdown.vue"

// Conditions for the counter
const count = ref(0)

// Function for example with ALERT
function showAlert(msg) {
  alert(msg)
}

// Conditions for dropdowns
const selectedFruit = ref(null)
const selectedFrameworks = ref([])
</script>

<template>
  <div class="p-6 space-y-12">
    <section>
      <h2 class="text-xl font-bold mb-4">Examples of buttons</h2>
      <div class="flex gap-4 flex-wrap">
        <Button
            label="Blue Small"
            color="blue"
            size="sm"
            icon="🔍"
            @click="showAlert('Blue small')"
        />
        <Button
            label="Red Medium"
            color="red"
            size="md"
            @click="showAlert('Red medium')"
        />
        <Button
            label="Green Large"
            color="green"
            size="lg"
            icon="✔"
            @click="showAlert('Green large')"
        />
        <Button
            label="Gray Reset"
            color="gray"
            size="md"
            @click="count = 0"
        />
      </div>
    </section>

    <section>
      <h2 class="text-xl font-bold mb-4">Counter</h2>
      <p class="text-lg mb-2">Value: {{ count }}</p>
      <div class="flex gap-4">
        <Button label="Increase" color="blue" @click="count++" />
        <Button label="Decrease" color="red" @click="count--" />
      </div>
    </section>

    <section>
      <h2 class="text-xl font-bold mb-4">Dropping list</h2>
      <div class="space-y-6">
        <div>
          <SearchableDropdown
              :items="['Apple', 'Banana', 'Cherry', 'Orange']"
              v-model="selectedFruit"
          />
          <p class="mt-2">You have chosen: <b>{{ selectedFruit }}</b></p>
        </div>

        <div>
          <SearchableDropdown
              :items="['Vue','React','Angular','Svelte']"
              multiple
              v-model="selectedFrameworks"
          >
            <template #option="{ item }">
              <span class="flex items-center gap-2">
                <span v-if="item === 'Vue'">🟢</span>
                <span v-else-if="item === 'React'">🔵</span>
                <span v-else-if="item === 'Angular'">🔺</span>
                <span v-else>🟣</span>
                {{ item }}
              </span>
            </template>
          </SearchableDropdown>
          <p class="mt-2">You have chosen several: <b>{{ selectedFrameworks.join(', ') }}</b></p>
        </div>
      </div>
    </section>
  </div>
</template>

<style scoped>

</style>
