<script setup>
/**
 * MonthSection Component
 * Groups TimelineCard components by month in a vertical layout
 * 
 * Props:
 * @param {string} month - The month/year header (e.g., "March 2024")
 * @param {Array} entries - Array of timeline entries for this month
 */
import TimelineCard from './TimelineCard.vue';

const props = defineProps({
  // Simple string prop for the month
  month: String,
  
  // Complex prop definition for entries
  entries: {
    // Specifies that this prop must be an Array
    type: Array,
    
    // Default value function that returns an empty array
    // Using a function ensures each component instance gets its own unique array
    // This prevents multiple instances from sharing the same array reference
    // If we used default: [] instead, all instances would share the same array
    default: () => []
  }
});
</script>

<template>
  <div class="month">
    <!-- Month Header -->
    <h3>{{ month }}</h3>
    
    <!-- Timeline entries for this month -->
    <div class="entries-container">
      <!-- Here, the props are passed to the TimelineCard component using the v-bind directive -->
      <TimelineCard
        v-for="(entry, index) in entries"
        :key="index"
        :title="entry.title"
        :description="entry.description"
        :type="entry.type"
        :link="entry.link"
      />
    </div>
  </div>
</template>

<style scoped>
.month {
  display: flex;
  flex-direction: column;
  gap: 10px;
  min-width: 200px;
}

.month h3 {
  margin: 0;
  padding: 10px;
  background-color: #6200ea;
  color: white;
  border-radius: 5px;
  text-align: center;
}

.entries-container {
  display: flex;
  flex-direction: column;
  gap: 10px;
}
</style>