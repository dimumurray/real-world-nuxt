<template>
    <div>
        <h1>Events for {{ user.user.name }}</h1>
        <EventCard
            v-for="event in event.events"
            :key="event.id"
            :event="event"
        />
        <template v-if="page !== 1">
            <router-link
                :to="{ name: 'event-list', query: { page: page - 1 } }"
                rel="prev"
                >Prev Page</router-link
            >
            <template v-if="hasNextPage"> | </template>
        </template>
        <router-link
            v-if="hasNextPage"
            :to="{ name: 'event-list', query: { page: page + 1 } }"
            rel="next"
            >Next Page</router-link
        >
    </div>
</template>

<script>
import EventCard from "@/components/EventCard.vue";
import { mapState, mapActions } from "vuex";

export default {
    components: {
        EventCard
    },
    created() {
        this.fetchEvents({
            perPage: this.perPage,
            page: this.page
        });
    },
    computed: {
        page() {
            return parseInt(this.$route.query.page) || 1;
        },
        perPage() {
            return parseInt(this.$route.query.perPage) || 3;
        },
        hasNextPage() {
            return this.event.eventsTotal > this.page * this.perPage;
        },
        ...mapState(["event", "user"])
    },
    methods: mapActions("event", ["fetchEvents"])
};
</script>
