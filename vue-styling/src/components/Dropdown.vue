<template>
  <div class="dropdown">
    <button
      :id="triggerId"
      ref="triggerRef"
      type="button"
      class="p-button p-button-secondary"
      aria-haspopup="true"
      :aria-expanded="isOpen"
      @click="toggle"
    >
      <slot name="trigger" />
    </button>

    <div
      v-show="isOpen"
      ref="contentRef"
      :id="contentId"
      class="dropdown-content"
      :class="{ 'is-open': isOpen }"
      :style="positionStyles"
      role="menu"
      :aria-labelledby="triggerId"
    >
      <slot />
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, onMounted, onBeforeUnmount } from 'vue'
import { useEventListener, useWindowSize } from '@vueuse/core'

const props = defineProps<{
  /** Unique identifier for the dropdown */
  id: string
  /** Placement of the dropdown */
  placement?: 'bottom-start' | 'bottom-end' | 'bottom'
}>()

const placement = computed(() => props.placement ?? 'bottom-start')

// Derived IDs for ARIA
const triggerId = `${props.id}-trigger`
const contentId = `${props.id}-content`

// Refs for positioning
const triggerRef = ref<HTMLButtonElement>()
const contentRef = ref<HTMLDivElement>()
const isOpen = ref(false)

// Position styles
const positionStyles = ref({
  top: '0px',
  left: '0px',
})

// Update dropdown position
function updatePosition() {
  if (!triggerRef.value || !contentRef.value || !isOpen.value) return

  const triggerRect = triggerRef.value.getBoundingClientRect()
  const contentRect = contentRef.value.getBoundingClientRect()

  let left = 0
  switch (placement.value) {
    case 'bottom-start':
      left = triggerRect.left
      break
    case 'bottom-end':
      left = triggerRect.right - contentRect.width
      break
    case 'bottom':
      left = triggerRect.left + (triggerRect.width - contentRect.width) / 2
      break
  }

  // Adjust for viewport boundaries
  const rightBoundary = window.innerWidth - contentRect.width
  left = Math.min(Math.max(0, left), rightBoundary)

  positionStyles.value = {
    top: `${triggerRect.bottom + 8}px`,
    left: `${left}px`,
  }
}

// Toggle dropdown
function toggle() {
  isOpen.value = !isOpen.value
  if (isOpen.value) {
    // Use nextTick to ensure content is rendered before measuring
    nextTick(updatePosition)
  }
}

// Event listeners for positioning
useEventListener(window, 'resize', updatePosition)
useEventListener(window, 'scroll', updatePosition)

// Close dropdown when clicking outside
useEventListener(document, 'click', (event: MouseEvent) => {
  const target = event.target as HTMLElement
  const isClickOutside = !triggerRef.value?.contains(target) && !contentRef.value?.contains(target)

  if (isClickOutside && isOpen.value) {
    isOpen.value = false
  }
})

// Handle escape key
useEventListener('keydown', (e: KeyboardEvent) => {
  if (e.key === 'Escape' && isOpen.value) {
    isOpen.value = false
  }
})
</script>

<style scoped>
.dropdown {
  position: relative;
  display: inline-block;
}

.dropdown-content {
  position: fixed;
  z-index: 50;
  min-width: 12rem;
  padding: var(--space-2);
  background: var(--surface-1);
  border: var(--border-2);
  border-radius: var(--radius-2);
  box-shadow: var(--shadow-3);
  opacity: 0;
  transform: translateY(-0.5rem);
  pointer-events: none;
}

.dropdown-content.is-open {
  opacity: 1;
  transform: translateY(0);
  pointer-events: auto;
}

/* Animation */
@media (prefers-reduced-motion: no-preference) {
  .dropdown-content {
    transition:
      opacity 150ms var(--ease-3),
      transform 150ms var(--ease-3);
  }
}

/* Default slot styling */
.dropdown-content :deep(.p-button) {
  display: block;
  width: 100%;
  text-align: left;
  background: transparent;
  border: 0;
  box-shadow: none;
}

.dropdown-content :deep(.p-button:hover) {
  background: var(--surface-2);
}
</style>
