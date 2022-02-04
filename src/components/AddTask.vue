<template>
  <form @submit="onSubmit">
    <div class="form-control">
      <label for="title">Task</label>
      <input type="text" name="title" placeholder="Add Task" v-model="title" />
    </div>

    <div class="form-control">
      <label for="data">Day & Time</label>
      <input
        type="text"
        name="date"
        placeholder="Add Day & Time"
        v-model="date"
      />
    </div>

    <div class="form-control form-control-check">
      <label for="reminder">Set Reminder</label>
      <input
        type="checkbox"
        name="reminder"
        class="input-checkbox"
        v-model="reminder"
      />
    </div>

    <button class="btn btn-block btn-primary">Save Task</button>
  </form>
</template>

<script>
export default {
  name: "AddTask",
  data() {
    return {
      title: "",
      date: "",
      reminder: false,
    };
  },
  methods: {
    onSubmit(e) {
      e.preventDefault();

      if (!this.title) {
        alert("Please add a task");
        return;
      }

      const newTask = {
        title: this.title,
        date: this.date,
        reminder: this.reminder,
      };

      this.$emit("add-task", newTask);

      this.title = "";
      this.date = "";
      this.reminder = false;
    },
  },
};
</script>

<style scoped>
form {
  margin-bottom: 1.5rem;
}
label,
input {
  display: block;
}

input[type="text"] {
  width: 100%;
  padding: 0.5rem;
  font-size: 1.15rem;
  border-radius: 5px;
  border: 2px solid rgb(196, 196, 196);
}

input::placeholder {
  font-family: "Poppins", sans-serif;
}

input:focus {
  outline: none;
}

.form-control {
  margin-bottom: 1rem;
}

.input-checkbox {
  height: 1.25rem;
}

.btn-block {
  display: block;
  width: 100%;
}

.btn-primary {
  background-color: rgb(52, 126, 255);
}
</style>
