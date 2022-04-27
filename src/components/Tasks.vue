<template>
  <div :key="task.id" v-for="task in tasks">
      <Task @toggle-reminder="toggleReminder" @delete-task="deleteTask" :task="task"/>
  </div>
</template>

<script>
import Task from './Task.vue'

export default {
    // eslint-disable-next-line
    name: 'Tasks',
    props: {
        tasks: Array
    },
    components: {
        Task
    },
    methods: {
        //could not capture data with inline emit to pass along again... tutorial used task.id again to pass along, but 
        // I wanted to keep the data sent along with the initial emit so using this method accomplished that
        deleteTask(id) {
            this.$emit('delete-task', id)
        },
        toggleReminder(id){
            this.$emit('toggle-reminder', id)
        }
    },
    emits: ['delete-task', 'toggle-reminder']

}
</script>

<style>

</style>