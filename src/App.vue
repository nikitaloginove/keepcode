<template>
  <div class="app">
    <h1 class="app__title">Мои задачи</h1>
    <div class="app__block">
      <input
        v-model="newTaskText"
        placeholder="Добавить новую задачу"
        @keyup.enter="addTask"
        class="app__block-input"
      />
      <button @click="addTask" class="app__block-button">Добавить</button>
    </div>

    <div class="app__filter-buttons">
      <button
        @click="filter = 'all'"
        :class="{ active: filter === 'all' }"
        class="app__filter-buttons-button"
      >
        Все
      </button>
      <button
        @click="filter = 'active'"
        :class="{ active: filter === 'active' }"
        class="app__filter-buttons-button"
      >
        Активные
      </button>
      <button
        @click="filter = 'completed'"
        :class="{ active: filter === 'completed' }"
        class="app__filter-buttons-button"
      >
        Выполненные
      </button>
    </div>

    <TaskList
      v-if="filter !== 'completed'"
      :tasks="filteredTasks"
      @delete-task="deleteTask"
      @update-task="updateTask"
      @toggle-complete="toggleComplete"
    />

    <CompletedTasks
      v-if="filter === 'completed'"
      :completed-tasks="tasks.filter((t) => t.completed)"
    />
  </div>
</template>

<script>
import TaskList from './components/TaskList.vue'
import CompletedTasks from './components/CompletedTasks.vue'

export default {
  name: 'App',
  components: { TaskList, CompletedTasks },
  data() {
    return {
      tasks: [],
      newTaskText: '',
      filter: 'all',
    }
  },
  created() {
    this.fetchTasks()
    console.log(localStorage.getItem('tasks'))
  },
  computed: {
    filteredTasks() {
      if (this.filter === 'all') {
        return this.tasks
      } else if (this.filter === 'completed') {
        return this.tasks.filter((task) => task.completed)
      } else if (this.filter === 'active') {
        return this.tasks.filter((task) => !task.completed)
      }
      return this.tasks
    },
  },
  methods: {
    fetchTasks() {
      const stored = localStorage.getItem('tasks')
      if (stored) {
        this.tasks = JSON.parse(stored)
      } else {
        fetch('https://jsonplaceholder.typicode.com/todos')
          .then((response) => response.json())
          .then((data) => {
            this.tasks = data.slice(0, 8).map((task) => ({
              id: task.id,
              text: task.title,
              isEditing: false,
              editText: '',
              completed: false,
            }))
            this.saveTasksToLocalStorage()
          })
      }
    },
    addTask() {
      if (this.newTaskText.trim()) {
        const newId = Date.now()
        this.tasks.push({
          id: newId,
          text: this.newTaskText.trim(),
          completed: false,
          isEditing: false,
          editText: '',
        })
        this.newTaskText = ''
        this.saveTasksToLocalStorage()
      }
    },
    deleteTask(id) {
      this.tasks = this.tasks.filter((task) => task.id !== id)
      this.saveTasksToLocalStorage()
    },
    updateTask(updatedTask) {
      const index = this.tasks.findIndex((t) => t.id === updatedTask.id)
      if (index !== -1) {
        this.tasks.splice(index, 1, updatedTask)
        this.saveTasksToLocalStorage()
      }
    },
    toggleComplete(task) {
      task.completed = !task.completed
      this.saveTasksToLocalStorage()
    },

    saveTasksToLocalStorage() {
      localStorage.setItem('tasks', JSON.stringify(this.tasks))
    },
  },
}
</script>

<style lang="scss" scoped>
.app {
  max-width: 800px;
  margin: auto;
  padding: 20px;

  &__title {
    text-align: center;
    font-size: 32px;
    margin-bottom: 20px;

    @media screen and (max-width: 767px) {
      font-size: 20px;
    }
  }

  &__block {
    display: flex;
    gap: 12px;
    margin-bottom: 20px;
  }

  &__block-input {
    flex-grow: 1;
    padding: 8px;
    border-radius: 4px;
    border: 1px solid black;
  }

  &__block-button {
    padding: 8px;
    background-color: #cce5ff;
    border-radius: 4px;
    border: 1px solid black;
    cursor: pointer;
    font-size: 12px;
    min-width: 120px;

    @media screen and (max-width: 767px) {
      min-width: 88px;
      font-size: 10px;
    }
  }

  &__filter-buttons {
    display: flex;
    justify-content: center;
    gap: 20px;
    margin-bottom: 20px;

    @media screen and (max-width: 767px) {
      justify-content: end;
      gap: 12px;
    }
  }

  &__filter-buttons-button {
    padding: 8px;
    background-color: #cce5ff;
    border: 1px solid black;
    border-radius: 4px;
    min-width: 120px;
    font-size: 12px;
    cursor: pointer;

    @media screen and (max-width: 767px) {
      min-width: 88px;
      font-size: 10px;
    }
  }
}
</style>
