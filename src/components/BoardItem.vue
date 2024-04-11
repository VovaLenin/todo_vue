<template>
  <div>
    <div class="title-container">
      <h1 class="board-title">{{ board.title }}</h1>
      <button class="plus-button" @click="$emit('addItem', board)">+</button>
    </div>
    <hr />
    <p v-if="board.items.length === 0">Нет активных задач</p>
    <!-- Для каждого элемента отображаем TaskItem компонент -->
    <task-item
      v-for="item in board.items"
      :key="item.id"
      :item="item"
      :board="board"
      @editItem="editItem"
      @removeItem="removeItem"
    ></task-item>
  </div>
</template>

<script>
import TaskItem from "./TaskItem.vue";

export default {
  components: {
    TaskItem,
  },
  props: ["board"],
  // методы editItem, removeItem переместить сюда из App
};
</script>
<!-- стили... -->
