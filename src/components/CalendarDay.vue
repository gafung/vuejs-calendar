<template>
    <div v-bind:class="classObject" @click="captureClick">
        {{ day.format('D') }}
        <ul class="event-list">
            <li v-for="event in events">{{ event.description }}</li>
        </ul>
    </div>
</template>

<script>
    export default {
        props: ['day'],
        computed: {
            events() {
                return this.$store.state.events.filter(event => event.date.isSame(this.day, 'days'));

            },

            classObject() {
                let eventFormDate = this.$store.state.eventFormDate;
                let eventFormActive = this.$store.state.eventFormActive;
                let today = this.$moment();
                return {
                    day: true,
                    today: this.day.isSame(today, 'days'),
                    past: this.day.isBefore(today, 'days'),
                    active: this.day.isSame(eventFormDate, 'days') && eventFormActive
                }
            }
        },
        methods: {
            captureClick(event) {
                this.$store.commit('eventFormPos', { x: event.clientX, y: event.clientY });
                this.$store.commit('eventFormActive', true);
                this.$store.commit('eventFormDate', this.day);
            }
        }
    }
</script>