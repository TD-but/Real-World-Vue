<template>
  <h1>Events For Good</h1>
  <div class="events">
    <EventCard v-for="event in events" :key="event.id" :event="event"/>
  </div>
</template>

<script setup>
import {onMounted, ref} from 'vue'
import EventCard from '@/components/EventCard.vue'
import EventService from '@/services/EventService'

const events = ref(null)

onMounted(() => {
  EventService.getEvents()
      .then((response) => {
        events.value = response.data
      })
      .catch((error) => {
        console.log(error)
      })
})
</script>

<style scoped>
.events {
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>
