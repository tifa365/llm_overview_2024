<script setup>
import { computed } from 'vue'; 

/**
 * FilterButton component for timeline filtering
 * It takes its props from the parent component, Timeline.
 * It also has its own props for the button label, active state, and color scheme.
 * It uses a computed property to filter the timeline data based on the active filter.
 * Props:
 * @param {string} label - Button text
 * @param {boolean} active - Whether this filter is currently active
 * @param {string} color - Button color scheme (show-all, open-source, api-only, or german)
 * @param {Function} onClick - Click handler function
 */
defineProps({
  label: String,
  // computing the filtered data based on the active filter
  active: Boolean,
  color: String,
  onClick: Function
});

// This computed property filters the timeline data based on the active filter. 
// It maps over each monthGroup and filters the entries according to the activeFilter value.
// If the activeFilter is 'all', all entries are included; otherwise, only entries matching the activeFilter type are returned.
const filteredData = computed(() => {
  return data.value.map(monthGroup => ({
    ...monthGroup,
    entries: monthGroup.entries.filter(entry => {
      if (activeFilter.value === 'all') return true
      return entry.type === activeFilter.value
    })
  }))
})

// Base classes shared by all buttons: To apply the same CSS styles to all buttons, we define 
// a base class that includes common styles like padding, border, and hover effects.
const baseClasses = "toggle-button";

// Classes mapping for different button states and types
const buttonTypeClasses = {
  'blue': 'show-all',
  'green': 'open-source',
  'red': 'api-only',
  'purple': 'german' 
};
</script>

<template>
  <button
    :class="[
      baseClasses, // Base CSS classes for all buttons
      buttonTypeClasses[color], // Class based on the button's color prop
      { active: active } // Object syntax to conditionally apply 'active' class
    ]"
    @click="onClick"
  >
    {{ label }}
  </button>
</template>

<style scoped>
/* Previous styles remain the same */
.toggle-button {
  padding: 8px 16px;
  border: none;
  color: black;
  border-radius: 5px;
  cursor: pointer;
  font-size: 14px;
  transition: background-color 0.3s, color 0.3s;
}

.toggle-button.show-all {
  background-color: #e3f2fd;
  border: 1px solid #2196f3;
  color: #2196f3;
}

.toggle-button.show-all.active {
  background-color: #2196f3;
  color: white;
}

.toggle-button.open-source {
  background-color: #e8f5e9;
  border: 1px solid #4caf50;
  color: #4caf50;
}

.toggle-button.open-source.active {
  background-color: #4caf50;
  color: white;
}

.toggle-button.api-only {
  background-color: #ffebee;
  border: 1px solid #f44336;
  color: #f44336;
}

.toggle-button.api-only.active {
  background-color: #f44336;
  color: white;
}

/* Added new styles for German filter */
.toggle-button.german {
  background-color: #f3e5f5;
  border: 1px solid #9c27b0;
  color: #9c27b0;
}

.toggle-button.german.active {
  background-color: #9c27b0;
  color: white;
}
</style>