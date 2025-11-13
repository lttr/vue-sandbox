<template>
  <div class="p-container p-stack">
    <div class="p-stack">
      <Card title="Card Title" subtitle="Optional subtitle" has-footer>
        <template #default> Card content goes here </template>

        <template #footer>
          <button>Action</button>
        </template>
      </Card>
      <Card title="Card Title" subtitle="Optional subtitle" has-footer>
        <template #default> Card content goes here </template>

        <template #footer>
          <button>Action</button>
        </template>
      </Card>
    </div>
    <div class="p-stack">
      <Dropdown id="user-menu">
        <template #trigger>Menu </template>

        <button class="p-button">Profile</button>
        <button class="p-button">Settings</button>
        <button class="p-button">Logout</button>
      </Dropdown>
    </div>
  </div>
</template>

<script setup lang="ts">
import Card from '@/components/Card.vue'
import Dropdown from '@/components/Dropdown.vue'
</script>
<style scoped></style>
