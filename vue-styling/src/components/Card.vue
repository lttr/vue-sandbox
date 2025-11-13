<script setup lang="ts">
interface Props {
  title?: string
  subtitle?: string
  hasFooter?: boolean
  hasHeader?: boolean
}

defineProps<Props>()
</script>

<template>
  <article class="card">
    <header v-if="hasHeader || title || subtitle" class="header">
      <h3 v-if="title" class="title">{{ title }}</h3>
      <p v-if="subtitle" class="subtitle">{{ subtitle }}</p>
    </header>

    <div class="content">
      <slot />
    </div>

    <footer v-if="hasFooter" class="footer">
      <slot name="footer" />
    </footer>
  </article>
</template>

<style scoped>
.card {
  max-width: var(--size-content-1);
  background: var(--surface-1);
  border-radius: var(--radius-2);
  box-shadow: var(--shadow-2);
  overflow: hidden;
}

.header {
  padding: var(--space-4);
  border-bottom: var(--border-2);
}

.title {
  font-size: var(--font-size-1);
  margin-bottom: var(--space-2);
}

.subtitle {
  color: var(--text-color-2);
  font-size: var(--font-size--1);
}

.content {
  padding: var(--space-4);
}

.footer {
  padding: var(--space-4);
  border-top: var(--border-2);
  background: var(--surface-2);
}
</style>
