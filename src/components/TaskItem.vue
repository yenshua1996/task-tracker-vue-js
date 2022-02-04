<template>
  <div class="task-content" @dblclick="$emit('toggle-reminder', task.id)">
    <div class="task-body">
      <h4 class="task-title">
        {{ task.title }}
      </h4>
      <p>{{ task.date }}</p>
    </div>

    <i @click="$emit('delete-task', task.id)" class="far fa-trash-alt"></i>
  </div>
</template>

<script>
export default {
  name: "TaskItem",
  props: {
    task: Object,
  },
};
</script>

<style scoped>
.task-content:hover {
  cursor: pointer;
}
</style>
