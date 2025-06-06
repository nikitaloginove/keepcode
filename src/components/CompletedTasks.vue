<template>
  <div class="completed-tasks">
    <h2 class="completed-tasks__title">Выполненные задачи</h2>
    <ul class="completed-tasks__list">
      <li v-for="task in completedTasks" :key="task.id" class="completed-tasks__item">
        {{ task.text }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'CompletedTasks',
  props: {
    completedTasks: Array,
  },
}
</script>

<style lang="scss" scoped>
.completed-tasks {
  margin-top: 40px;

  &__title {
    font-size: 32px;
    margin-bottom: 20px;
    text-align: center;

    @media screen and (max-width: 767px) {
      font-size: 20px;
    }
  }

  &__list {
    display: flex;
    flex-direction: column;
    gap: 12px;
    list-style: none;
    text-align: start;
  }

  &__item {
    background-color: #fff;
    border-radius: 4px;
    padding: 8px 20px;
    border: 1px solid black;

    @media screen and (max-width: 767px) {
      font-size: 12px;
    }
  }
}
</style>
