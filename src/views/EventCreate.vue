<template>
   <div>
       <!--<h1>Create an Event, {{ user.name }}</h1>
       <p>This Event was created by {{ user.id }}</p>
       <p>There are {{ catLength }} categories</p>
       <p>Done Todos is :  {{ doneTodos.length }}</p>
       <p>Active Todos is :  {{ activeTodosCount.length }}</p>
       <p>Todo By Id :  {{ getTodosById(1) }}</p>
       <ul>
           <li v-for="cat in categories" :key="cat">{{cat}}</li>
       </ul>-->
       <h3>Create Form:</h3>
       <form @submit.prevent="createEvent">
           <label Select a category></label>
            <select v-model="event.category">
                <option v-for="cat in categories" :key="cat">{{ cat }}</option>
            </select>

            <h3>Name & describe your event</h3>
            <div class="field">
                <label>Title</label>
                <input v-model="event.title" type="text" placeholder="Add an event title"/>
            </div>

            <div class="field">
                <label>Description</label>
                <input v-model="event.description" type="text" placeholder="Add an event description"/>
            </div>

            <h3>Where is your event?</h3>
            <div class="field">
                <label>Location</label>
                <input v-model="event.location" type="text" placeholder="Add an event location"/>
            </div>

            <h3>when is your event?</h3>
            <div class="field">
                <label>Date</label>
                <DatePicker v-model="event.date" type="text" placeholder="select a date"/>
            </div>

            <div class="field">
                <label>Select a time</label>
                <select v-model="event.time">
                    <option v-for="time in times" :key="time">{{ time }}</option>
                </select>
            </div>

            <input type="submit" class="button -fill-gradient" value="Submit"/>
       </form>
   </div>
</template>

<script>
//import { mapState, mapGetters } from 'vuex'
import DatePicker from 'vuejs-datepicker'

export default {
    components: {
        DatePicker,
    },
    computed: {
        //...mapGetters (['catLength', 'doneTodos', 'activeTodosCount', 'getTodosById']),
        //...mapState (['user', 'categories'])
    },
    data() {
        const times = []
        for (let i = 0; i <= 24; i++) {
            times.push(i + ':00')
        }
        return {
            times,
            categories: this.$store.state.categories,
            event : {},
        }
    },
    created (){
        this.createFreshEventObject();
    },
    methods: {
         createEvent() {
            this.$store.dispatch('event/createEvent', this.event)
            .then( () => {
                this.$router.push({
                    name: 'event-show',
                    params: {id: this.event.id}
                })
                 this.event = this.createFreshEventObject();
            })
            .catch( () => {
                console.log('There was a problem creating your event')
            })
            
        },
        createFreshEventObject () {
            const user = this.$store.state.user.user
            const id = Math.floor(Math.random() * 1000000)

            this.event = {
                id: id,
                user: user,
                category: '',
                organizer: user,
                title: '',
                description: '',
                location: '',
                date: '',
                time: '',
                attendees: []
            }
        },
    }
}
</script>

<style scoped>
.field {
    margin-bottom: 24px;
}
</style>