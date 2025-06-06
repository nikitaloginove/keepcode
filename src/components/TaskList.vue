<template>
  <div class="task-list">
    <TaskItem
      v-for="task in tasks"
      :key="task.id"
      :task="task"
      @delete-task="$emit('delete-task', $event)"
      @update-task="$emit('update-task', $event)"
      @toggle-complete="$emit('toggle-complete', $event)"
    />
  </div>
</template>

<script>
import TaskItem from './TaskItem.vue'

export default {
  name: 'TaskList',
  components: { TaskItem },
  props: {
    tasks: Array,
  },
}
</script>

<style scoped>
.task-list {
  display: flex;
  flex-direction: column;
  gap: 10px;
}
</style>
