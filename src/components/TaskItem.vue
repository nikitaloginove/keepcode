<template>
  <div class="task-item">
    <div v-if="isEditing" class="task-item__edit">
      <input v-model="editText" @keyup.enter="saveEdit" class="task-item__edit-input" />
      <div class="task-item__edit-buttons">
        <button @click="saveEdit" class="task-item__edit-button">Сохранить</button>
        <button @click="cancelEdit" class="task-item__edit-button task-item__edit-button-delete">
          Отмена
        </button>
      </div>
    </div>
    <div v-else class="task-item__view">
      <div class="task-item__view-block">
        <input
          type="checkbox"
          :checked="task.completed"
          @change="$emit('toggle-complete', task)"
          class="task-item__view-input"
        />
        <span :class="{ completed: task.completed }" class="task-item__view-text">{{
          task.text
        }}</span>
      </div>
      <div class="task-item__view-buttons">
        <button @click="startEdit" class="task-item__view-button">Редактировать</button>
        <button
          @click="$emit('delete-task', task.id)"
          class="task-item__view-button task-item__view-button-delete"
        >
          Удалить
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TaskItem',
  props: {
    task: Object,
  },
  data() {
    return {
      isEditing: false,
      editText: '',
    }
  },
  methods: {
    startEdit() {
      this.isEditing = true
      this.editText = this.task.text
    },
    saveEdit() {
      if (this.editText.trim()) {
        const updatedTask = { ...this.task, text: this.editText.trim(), isEditing: false }
        this.$emit('update-task', updatedTask)
        this.isEditing = false
      }
    },
    cancelEdit() {
      this.isEditing = false
    },
  },
}
</script>

<style lang="scss" scoped>
.task-item {
  display: flex;
  align-items: center;
  gap: 10px;

  &__edit {
    display: flex;
    justify-content: space-between;
    width: 100%;
  }

  &__edit-input {
    width: 100%;
    margin-right: 12px;
    border-radius: 4px;
    padding: 4px;
    border: 1px solid black;
  }

  &__edit-buttons {
    display: flex;
    gap: 12px;
    font-size: 12px;
  }

  &__edit-button {
    padding: 8px;
    background-color: #cce5ff;
    border-radius: 4px;
    cursor: pointer;
  }

  &__edit-button-delete {
    background-color: #ffcccc;
  }

  &__view {
    display: flex;
    justify-content: space-between;
    width: 100%;
  }

  &__view-block {
    display: flex;
    flex-direction: row;
    align-items: center;
    gap: 4px;
  }

  &__view-input {
    width: 20px;
    height: 20px;
    cursor: pointer;

    @media screen and (max-width: 767px) {
      width: 12px;
      height: 12px;
    }
  }

  &__view-text {
    margin-left: 12px;
    margin-right: 12px;
    font-size: 16px;

    @media screen and (max-width: 767px) {
      font-size: 12px;
    }
  }

  &__view-buttons {
    display: flex;
    gap: 12px;
    font-size: 12px;
  }

  &__view-button {
    padding: 8px;
    background-color: #cce5ff;
    border-radius: 4px;
    cursor: pointer;
  }

  &__view-button-delete {
    background-color: #ffcccc;
  }
}

button {
  min-width: 120px;
  max-height: fit-content;
  border: 1px solid black;

  @media screen and (max-width: 767px) {
    min-width: 88px;
    font-size: 10px;
  }
}
</style>
