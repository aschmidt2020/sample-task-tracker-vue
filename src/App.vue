<template>
  <div class="container">
    <Header @toggle-add-task="toggleAddTask" :showAddTask="showAddTask" />
    <div v-if="showAddTask">
      <AddTask @add-task="addTask" :tasks="tasks" />
    </div>
    <Tasks
      @toggle-reminder="toggleReminder"
      @delete-task="deleteTask"
      :tasks="tasks"
    />
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import Tasks from "./components/Tasks.vue";
import AddTask from "./components/AddTask.vue";

export default {
  name: "App",
  components: {
    Header,
    Tasks,
    AddTask,
  },
  data() {
    return {
      tasks: [],
      showAddTask: false,
    };
  },
  methods: {
    async addTask(task) {
      let response = await fetch("api/tasks", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(task),
      });

      let data = await response.json();
      console.log(data);
      // this.tasks = [...this.tasks, data] --> can use this to update UI or make another call
      this.tasks = await this.fetchTasks();
    },
    toggleAddTask() {
      this.showAddTask = !this.showAddTask;
    },
    async deleteTask(id) {
      if (confirm("Are you sure you would like to delete this?")) {
        let response = await fetch(`api/tasks/${id}`, {
          method: "DELETE",
        });
        if (response.status === 200) {
          //this.tasks = this.tasks.filter((task) => task.id !== id)
          this.tasks = await this.fetchTasks();
        } else {
          alert("Error deleting task");
        }
      }
    },
    async toggleReminder(id) {
      let taskToToggle = await this.fetchTask(id)
      let updatedTask = {...taskToToggle, reminder: !taskToToggle.reminder} 
      
      let response = await fetch(`api/tasks/${id}`, {
          method: "PUT",
           headers: {
          "Content-Type": "application/json",
          },
          body: JSON.stringify(updatedTask),
        });

        if (response.status === 200) {
          //could use map function to just update UTI if needed
          this.tasks = await this.fetchTasks();
        } else {
          alert("Error toggling task");
        }
        
    },
    async fetchTasks() {
      let response = await fetch("api/tasks");
      let data = await response.json();
      return data;
    },
    async fetchTask(id) {
      let response = await fetch(`api/tasks/${id}`);
      let data = await response.json();
      return data;
    },
  },
  async created() {
    this.tasks = await this.fetchTasks();
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap");
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: "Poppins", sans-serif;
}
.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}
.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}
.btn:focus {
  outline: none;
}
.btn:active {
  transform: scale(0.98);
}
.btn-block {
  display: block;
  width: 100%;
}
</style>
