<template>
  <div v-if="event">
    <h1>{{ event.title }}</h1>
    <!-- prettier-ignore -->
    <nav>
      <router-link :to="{ name: 'event-details'}">Details</router-link> |
      <router-link :to="{ name: 'event-register'}">Register</router-link> |
      <router-link :to="{ name: 'event-edit'}">Edit</router-link>
    </nav>
    <RouterView :event="event" />
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue'
import EventService from '@/services/EventService'

const event = ref(null)

const props = defineProps({
  id: {
    required: true,
  },
})

onMounted(() => {
  EventService.getEvent(props.id)
    .then((response) => {
      event.value = response.data
      console.log('LOADED THE EVENT DATA' + event.value)
    })
    .catch((error) => {
      console.log(error)
    })
})
</script>
