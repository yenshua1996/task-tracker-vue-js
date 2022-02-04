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
  name: "App",
  components: {
    Header,
    TaskList,
    AddTask,
  },
  emits: ["add-task"],
  data() {
    return {
      tasks: [],
      showAddTask: false,
    };
  },
  methods: {
    toggleAddTask() {
      this.showAddTask = !this.showAddTask;
    },
    addTask(task) {
      this.tasks = [...this.tasks, task];
    },
    deleteTask(id) {
      if (confirm("Are you sure?")) {
        this.tasks = this.tasks.filter((task) => task.id !== id);
      }
    },
    toggleReminder(id) {
      this.tasks = this.tasks.map((task) =>
        task.id === id ? { ...task, reminder: !task.reminder } : task
      );
    },
  },
  created() {
    this.tasks = [
      {
        id: 1,
        title: "Appointment to dentist",
        date: "March 1st 2022",
        reminder: true,
      },
      {
        id: 2,
        title: "PTA school meeting",
        date: "May 3rd 2022",
        reminder: true,
      },
      {
        id: 3,
        title: "Car monthly inspection",
        date: "April 15th 2022",
        reminder: true,
      },
      {
        id: 4,
        title: "Create new application",
        date: "March 15th 1st 2022",
        reminder: false,
      },
      {
        id: 5,
        title: "Follow up check-up",
        date: "June 1st 1st 2022",
        reminder: false,
      },
    ];
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
  border: 2px solid rgb(162, 162, 162);
  border-radius: 5px;
  padding: 1rem;
}

.container {
  margin: 0 auto;
  width: 80%;
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
