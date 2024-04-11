<template>
  <ul class="board-container">
    <li
      class="board"
      v-for="board in boards"
      :key="board.id"
      @drop.prevent="dropBoard($event, board)"
      @dragover.prevent="dragOver($event)"
    >
      <!-- Отправляем доску в качестве props в BoardItem -->
      <board-item
        :board="board"
        @editItem="editItem"
        @removeItem="removeItem"
      ></board-item>
    </li>
  </ul>
</template>

<script>
import BoardItem from "./BoardItem.vue";

export default {
  components: {
    BoardItem,
  },
  props: ["boards"],
  // методы dropBoard, dragOver, editItem, removeItem переместить сюда из App
};
</script>
<!-- стили... -->
