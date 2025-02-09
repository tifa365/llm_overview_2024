<script setup>
/**
 * TimelineCard Component
 * Displays a single AI model release event with type-based styling
 * 
 * Props:
 * @param {string} title - Name of the AI model
 * @param {string} description - Description of the model release
 * @param {string} type - Type of release (api, weights, german)
 * @param {string} [link] - Optional link to the model
 */
// This defines what data the component can receive from its parent.
const props = defineProps({
  title: String,
  description: String,
  type: String,
  german: Boolean,
  link: {
    type: String,
    default: null
  }
});

// Computing border and badge styling based on ...
const getTypeStyles = () => {
  if (props.german) {  // Check for german property first
    return {
      border: 'border-l-4 border-purple-500',
      background: 'bg-purple-50',
      badge: {
        classes: 'tag german',
        text: 'German Model'
      }
    };
  }
  
  switch (props.type) {
    case 'api':
      return {
        border: 'border-l-4 border-red-500',
        background: 'bg-red-50',
        badge: {
          classes: 'tag api-only',
          text: 'API Only'
        }
      };
    case 'weights':
      return {
        border: 'border-l-4 border-green-500',
        background: 'bg-green-50',
        badge: {
          classes: 'tag open-source',
          text: 'Open Weights'
        }
      };
    default:
      return {
        border: '',
        background: '',
        badge: null
      };
  }
};
</script>

// The : prefix is Vue shorthand for v-bind:class This is dynamic: The class will change based on what getTypeStyles().border returns
<template>
  <div 
    class="event" 
    :class="[
      getTypeStyles().border,
      getTypeStyles().background
    ]"
  >
    <h4>{{ title }}</h4>
    <p>
      {{ description }}
      <a 
        v-if="link" 
        :href="link" 
        target="_blank" 
        rel="noopener"
        class="learn-more"
      >Learn more</a>
    </p>
    <span 
      v-if="getTypeStyles().badge"
      :class="getTypeStyles().badge.classes"
    >
      {{ getTypeStyles().badge.text }}
    </span>
  </div>
</template>

<style scoped>
.event {
  background-color: white;
  padding: 10px;
  border-radius: 5px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  position: relative;
  transition: all 0.3s ease;
  width: 100%;           
  min-height: fit-content;
}

.event.bg-red-50 {
  background-color: #fee2e2;
}

.event.bg-green-50 {
  background-color: #e8f5e9;
}

.event.bg-purple-50 {
  background-color: #f3e5f5;
}

.event h4 {
  margin: 0;
  font-size: 14px;
}

.event p {
  margin: 5px 0 0;
  font-size: 12px;
  color: #666;
  white-space: normal;   /* Add this */
  word-wrap: break-word; /* Add this */
  overflow-wrap: break-word; /* Add this */
}

.tag {
  display: inline-block;
  padding: 2px 5px;
  font-size: 10px;
  border-radius: 3px;
  margin-top: 5px;
}

.tag.api-only {
  background-color: #f44336;
  color: white;
}

.tag.open-source {
  background-color: #4caf50;
  color: white;
}

.tag.german {
  background-color: #9c27b0;
  color: white;
}

/* Hover effects */
.event:hover {
  transform: scale(1.02);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15);
}

a {
  color: #4caf50;
  text-decoration: none;
}

a:hover {
  text-decoration: underline;
}
</style>