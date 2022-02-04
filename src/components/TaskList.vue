<template>
  <div class="task">
    <div
      :class="[task.reminder ? 'reminder' : '', 'task-item']"
      v-for="task in tasks"
      :key="task.id"
    >
      <TaskItem
        :task="task"
        @delete-task="$emit('delete-task', task.id)"
        @toggle-reminder="$emit('toggle-reminder', task.id)"
      />
    </div>
  </div>
</template>

<script>
import TaskItem from "./TaskItem.vue";

export default {
  name: "TaskList",
  props: {
    tasks: Array,
  },
  components: {
    TaskItem,
  },
  emits: ["delete-task", "toggle-reminder"],
};
</script>

<style scoped>
.task-item {
  background-color: rgb(231, 231, 231);
  margin-bottom: 0.5rem;
  padding: 0.5rem;
  border-radius: 5px;
  border-left: 4px solid rgba(0, 0, 0, 0);
}

.task-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.task-title {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.reminder {
  border-left: 4px solid green;
}
</style>
