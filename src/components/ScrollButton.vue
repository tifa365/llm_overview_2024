<script setup>
const props = defineProps({
  onClick: {
    type: Function,
    required: true
  }
})
</script>


<template>
    <button 
      class="w-10 h-10 rounded-full bg-black hover:bg-gray-800 flex items-center justify-center text-white shadow-sm transition-all absolute right-4 top-1/2 -translate-y-1/2 z-10"
      @click="onClick"
    >
      <!-- Using a simple chevron character if you don't have an icon library -->
      <span class="text-xl">›</span>
    </button>
</template>

