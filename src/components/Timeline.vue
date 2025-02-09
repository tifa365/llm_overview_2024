<template>
  <!-- Main wrapper with scaling for better overview -->
  <div class="timeline-wrapper">
    <!-- Primary container with scroll functionality -->
    <div class="timeline-container">
      <!-- Filter controls -->
      <div class="toggle-container">
        <FilterButton
          label="Show All"
          :active="activeFilter === 'all'"
          @click="setFilter('all')"
          color="blue"
        />
        <FilterButton
          label="Open Weights Only"
          :active="activeFilter === 'weights'"
          @click="setFilter('weights')"
          color="green"
        />
        <FilterButton
          label="API Only"
          :active="activeFilter === 'api'"
          @click="setFilter('api')"
          color="red"
        />
      </div>

      <!-- Scrollable timeline content -->
      <div class="timeline-scroll" ref="scrollContainer">
        <div class="timeline-grid">
          <MonthSection
            v-for="(monthGroup, index) in filteredData"
            :key="index"
            :month="monthGroup.month"
            :entries="monthGroup.entries"
            class="month-section"
          />
        </div>
      </div>
    </div>
    
    <!-- Scroll controls -->
    <div class="scroll-controls">
      <button 
        class="scroll-button left" 
        @click="scrollLeft"
        aria-label="Scroll left"
      >
        <span class="arrow">←</span>
      </button>
      <button 
        class="scroll-button right" 
        @click="scrollRight"
        aria-label="Scroll right"
      >
        <span class="arrow">→</span>
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import FilterButton from './FilterButton.vue'
import MonthSection from './MonthSection.vue'
import { timelineData } from '../data/timelineData.js'

// Initialize with timeline data
const data = ref(timelineData)
const scrollContainer = ref(null)

// Scroll control functions
// Amount to scroll per button click (approximately 2 months worth of content)
const scrollAmount = 700 

/**
 * Scrolls the timeline to the left by the defined scroll amount
 * Uses smooth scrolling behavior for better user experience
 */
const scrollLeft = () => {
  if (scrollContainer.value) {
    scrollContainer.value.scrollBy({
      left: -scrollAmount,
      behavior: 'smooth'
    })
  }
}

/**
 * Scrolls the timeline to the right by the defined scroll amount
 * Uses smooth scrolling behavior for better user experience
 */
const scrollRight = () => {
  if (scrollContainer.value) {
    scrollContainer.value.scrollBy({
      left: scrollAmount,
      behavior: 'smooth'
    })
  }
}

// Active filter state management
const activeFilter = ref('all')

/**
 * Updates the active filter state to control which entries are displayed.
 * @param {string} filter - The filter value to apply ('all', 'weights', or 'api')
 */
const setFilter = (filter) => {
  activeFilter.value = filter
}

/**
 * Computed property that filters timeline data based on active filter
 */
const filteredData = computed(() => {
  return data.value.map(monthGroup => ({
    ...monthGroup,
    entries: monthGroup.entries.filter(entry => {
      if (activeFilter.value === 'all') return true
      return entry.type === activeFilter.value
    })
  }))
})
</script>

<style scoped>
/* Main wrapper with scaling */
.timeline-wrapper {
  position: relative;
  width: 100%;
  height: 100vh;
  overflow: hidden;
}

/* Primary container with scaling */
.timeline-container {
  transform: scale(0.65);
  transform-origin: top left;
  width: 153.85%; /* Compensate for scale (100/0.65) */
  height: 153.85%; /* Compensate for scale */
  padding: 0;
  display: flex;
  flex-direction: column;
}

/* Filter buttons container */
.toggle-container {
  display: flex;
  gap: 1rem;
  justify-content: center;
  margin-bottom: 1.5rem;
  position: sticky;
  top: 0;
  z-index: 10;
  background: white;
  padding: 0.5rem 0;
}

/* Scrollable content area */
.timeline-scroll {
  flex: 1;
  overflow-x: scroll;
  overflow-y: scroll;
  scrollbar-width: thin;  /* For Firefox */
  white-space: nowrap;     /* Prevent content from wrapping */
}



.timeline-scroll::-webkit-scrollbar {
  width: 8px;  /* For Chrome/Safari */
  height: 0px;   /* Hide horizontal scrollbar */
}

/* Hide ONLY horizontal scrollbar */
.timeline-scroll::-webkit-scrollbar-horizontal {
  display: none;          /* Hide horizontal scrollbar */
}

.timeline-scroll::-webkit-scrollbar-thumb {
  background-color: #888;
  border-radius: 3px;
}

.timeline-scroll::-webkit-scrollbar-thumb:hover {
  background-color: #555;
}

/* Grid layout for month sections */
.timeline-grid {
  display: flex;
  gap: 1rem;
  min-height: min-content;
  flex-wrap: nowrap;
  margin: 0;
  padding: 0;
}

/* Individual month section */
.month-section {
  flex: 0 0 300px;
  padding: 1rem;
  border-right: 1px solid #e5e7eb;
  overflow-y: auto;
  overflow-x: hidden;

  &::-webkit-scrollbar {
    width: 6px;
    background-color: #f5f5f5;
  }

  &::-webkit-scrollbar-thumb {
    background-color: #888;
    border-radius: 3px;
  }

  &::-webkit-scrollbar-thumb:hover {
    background-color: #555;
  }
}

/* Scroll controls styling */
.scroll-controls {
  position: absolute;
  bottom: 2rem;  /* Moved further down */
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  gap: 1rem;
  z-index: 20;
}

.scroll-button {
  background: rgba(255, 255, 255, 0.9);
  border: 1px solid #e5e7eb;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all 0.2s ease;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);

  &:hover {
    background: white;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  }

  &:active {
    transform: translateY(1px);
  }
}

.arrow {
  font-size: 1.2rem;
  line-height: 1;
}

/* Timeline element spacing */
.timeline {
  display: flex;
  flex-direction: row;
  padding: 1rem;
  gap: 1rem;
}
</style>