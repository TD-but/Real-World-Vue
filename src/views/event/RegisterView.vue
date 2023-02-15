<template>
  <p>Register for the event here</p>
  <button @click="register">Register Me</button>
</template>

<script setup>
import router from '@/router'

defineProps({
  event: {
    required: true,
  },
})

function register() {
  // Call to API
  // If registered then redirect to event details
  router.push({
    name: 'event-details',
  })
}
</script>

<style lang="scss" scoped></style>
