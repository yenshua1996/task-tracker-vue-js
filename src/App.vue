<!-- Component Template-->
<template>
  <div class="container">
    <Header
      message="Task Tracker"
      @btn-click="toggleAddTask"
      :showAddTask="showAddTask"
    />
    <div v-if="showAddTask">
      <AddTask @add-task="addTask" />
    </div>
    <TaskList
      :tasks="tasks"
      @delete-task="deleteTask"
      @toggle-reminder="toggleReminder"
    />
  </div>
</template>

<!-- Component Logic-->
<script>
import Header from "./components/Header.vue";
import TaskList from "./components/TaskList.vue";
import AddTask from "./components/AddTask.vue";

export default {
  // Component Name
  name: "App",
  // Register Component
  components: {
    Header,
    TaskList,
    AddTask,
  },
  // Register Emit
  emits: ["add-task"],
  // State
  data() {
    return {
      tasks: [],
      showAddTask: false,
    };
  },
  // Component Methods
  methods: {
    toggleAddTask() {
      this.showAddTask = !this.showAddTask;
    },

    async addTask(task) {
      const response = await fetch("http://localhost:5000/tasks", {
        method: "POST",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(task),
      });

      const data = await response.json();

      this.tasks = [...this.tasks, data];
    },

    async deleteTask(id) {
      if (confirm("Are you sure?")) {
        const response = await fetch(`http://localhost:5000/tasks/${id}`, {
          method: "DELETE",
        });

        response.status === 200
          ? (this.tasks = this.tasks.filter((task) => task.id !== id))
          : alert("Error encountered when deleting task.");
      }
    },

    async toggleReminder(id) {
      const taskToToggle = await this.fetchTask(id);

      const updateTask = { ...taskToToggle, reminder: !taskToToggle.reminder };

      const response = await fetch(`http://localhost:5000/tasks/${id}`, {
        method: "PUT",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(updateTask),
      });

      const data = await response.json();

      this.tasks = this.tasks.map((task) =>
        task.id === id ? { ...task, reminder: data.reminder } : task
      );
    },

    async fetchTasks() {
      const response = await fetch("http://localhost:5000/tasks");

      const data = await response.json();

      return data;
    },

    async fetchTask(id) {
      const response = await fetch(`http://localhost:5000/tasks/${id}`);

      const data = await response.json();

      return data;
    },
  },
  // Life Cycle methods
  async created() {
    this.tasks = await this.fetchTasks();
  },
};
</script>

<!-- Component Style-->
<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600&display=swap");

* {
  box-sizing: border-box;
  padding: 0;
  margin: 0;
}

body {
  font-family: "Poppins", sans-serif;
  line-height: 1.6;
  padding: 1rem;
}

h1 {
  line-height: 1.2;
  font-weight: 400;
  text-align: center;
  padding: 2rem 0;
}

#app {
  border-radius: 5px;
  padding: 1rem;
  margin: 0 auto;
  width: 50%;
}

.btn {
  padding: 0.5rem 2rem;
  font-family: "Poppins", sans-serif;
  border: none;
  border-radius: 5px;
  border: 1px solid rgba(68, 68, 68, 0);
  transition: all 200ms ease-in-out;
  color: #fff;
}

.btn:focus {
  outline: none;
}

.btn:hover {
  cursor: pointer;
  transform: translateY(-2px);
  border: 1px solid #444;
}
</style>
