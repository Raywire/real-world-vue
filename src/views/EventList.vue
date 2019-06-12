<template>
  <div>
    <h1>Event for {{ user.user.name }}</h1>
    <EventCard v-for="event in event.events" :key="event.id" :event="event" />
    <template v-if="page != 1">
      <router-link
        class="pagination"
        :to="{ name: 'event-list', query: { page: page - 1 } }"
        rel="prev"
        >Previous Page</router-link
      >
    </template>
    <template v-if="hasNextPage && page != 1">
      <span class="padded">|</span>
    </template>
    <router-link
      class="pagination"
      v-if="hasNextPage"
      :to="{ name: 'event-list', query: { page: page + 1 } }"
      rel="next"
      >Next Page</router-link
    >
  </div>
</template>
<script>
import EventCard from "@/components/EventCard.vue";
import { mapState } from "vuex";
import store from "@/store/store";

function getPageEvents(routeTo, next) {
  const currentPage = parseInt(routeTo.query.page) || 1;
  store
    .dispatch("event/fetchEvents", {
      page: currentPage
    })
    .then(() => {
      routeTo.params.page = currentPage;
      next();
    });
}
export default {
  props: {
    page: {
      type: Number,
      required: true
    }
  },
  components: {
    EventCard
  },
  beforeRouteEnter(routeTo, routeFrom, next) {
    getPageEvents(routeTo, next);
  },
  beforeRouteUpdate(routeTo, routeFrom, next) {
    getPageEvents(routeTo, next);
  },
  computed: {
    hasNextPage() {
      return this.event.eventsTotal > this.page * this.event.perPage;
    },
    ...mapState(["event", "user"])
  }
};
</script>
<style scoped>
.padded {
  margin: 0 5px;
}
.pagination {
  text-decoration: none;
}
.pagination:hover {
  text-decoration: underline;
}
</style>
