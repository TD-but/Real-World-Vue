<template>
  <h1>Events For Good</h1>
  <div class="events">
    <EventCard v-for="event in events" :key="event.id" :event="event" />

    <div class="pagination">
      <div id="page-links">
        <span
          id="page-link"
          @click="gotoPage(n)"
          v-for="n in totalPages"
          :key="n"
          >{{ n }}</span
        >
      </div>

      <router-link
        id="page-prev"
        :to="{ name: 'event-list', query: { page: page - 1 } }"
        rel="prev"
        v-show="page !== 1"
      >
        &#60; Previous
      </router-link>

      <router-link
        id="page-next"
        :to="{ name: 'event-list', query: { page: page + 1 } }"
        rel="next"
        v-show="hasNextPage"
      >
        Next &#62;
      </router-link>
    </div>
  </div>
</template>

<script setup>
import { computed, onMounted, ref, watchEffect } from 'vue'
import EventCard from '@/components/EventCard.vue'
import EventService from '@/services/EventService'
import router from '@/router'

const events = ref(null)
const totalEvents = ref(null)

const props = defineProps({
  page: Number,
})

const hasNextPage = computed(() => {
  return props.page < totalPages.value
})

const totalPages = computed(() => {
  return Math.ceil(totalEvents.value / 2)
})

function gotoPage(pageNumber) {
  router.push({ name: 'event-list', query: { page: pageNumber } })
}

onMounted(() => {
  watchEffect(() => {
    events.value = null
    EventService.getEvents(2, props.page)
      .then((response) => {
        events.value = response.data
        totalEvents.value = response.headers['x-total-count']
      })
      .catch((error) => {
        console.log(error)
      })
  })
})
</script>

<style scoped>
.events {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.pagination {
  display: flex;
  width: 290px;
  justify-content: space-between;
}

.pagination a {
  text-decoration: none;
  color: #2c3e50;
}

#page-prev {
  text-align: left;
  width: 100px;
  margin-right: auto;
  z-index: 1;
}

#page-next {
  text-align: right;
  width: 100px;
  margin-left: auto;
  z-index: 1;
}

#page-links {
  display: block;
  position: absolute;
  width: inherit;
  z-index: 0;
}

#page-link {
  margin-left: 6px;
  margin-right: 6px;
  cursor: pointer;
}
</style>
