<template>
  <li
    class="board"
    @drop.prevent="dropBoard(board)"
    @dragover.prevent="dragOver($event)"
  >
    <div class="title-container">
      <h1 class="board-title">{{ board.title }}</h1>
      <button class="plus-button" @click="addItem(board)">+</button>
    </div>
    <hr />
    <p v-if="board.items.length === 0">Нет активных задач</p>
    <ul>
      <TaskItem
        v-for="item in board.items"
        :item="item"
        :board="board"
        :key="item.id"
        @edit-item="editItem(item)"
        @stop-editing="stopEditing"
        :modelValue="modelValue"
        @update:modelValue="$emit('update:modelValue', $event)"
        @is-editing-item="isEditingItem"
        @remove-item="removeItem"
        @drag-start="dragStart"
        @drag-end="dragEnd"
        @drag-over="dragOver"
        @drag-leave="dragLeave"
        @drop="drop"
      />
    </ul>
  </li>
</template>

<script>
import TaskItem from "./TaskItem.vue";

export default {
  components: {
    TaskItem,
  },
  props: ["board", "modelValue"],
  methods: {
    editItem(item) {
      this.$emit("edit-item", item);
    },
    stopEditing() {
      this.$emit("stop-editing");
    },
    isEditingItem(item) {
      this.$emit("is-editing-item", item);
    },
    removeItem(payload) {
      this.$emit("remove-item", payload);
    },
    addItem(board) {
      this.$emit("add-item", board);
    },
    dragStart(payload) {
      this.$emit("drag-start", payload);
    },
    dragEnd(event) {
      this.$emit("drag-end", event);
    },
    dragOver(event) {
      this.$emit("drag-over", event);
    },
    dragLeave(event) {
      this.$emit("drag-leave", event);
    },
    drop(payload) {
      this.$emit("drop", payload);
    },
    dropBoard(board) {
      this.$emit("drop-board", board);
    },
  },
};
</script>

<style>
.title-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.title-container:hover .plus-button {
  display: inline-flex;
}

.title-container:hover .plus-button {
  display: inline-flex;
}

.plus-button {
  width: 30px;
  height: 30px;
  font-size: 28px;
  line-height: 1;
  border: 1px solid #ccc;
  background-color: #f8f8f8;
  border-radius: 50%;
  cursor: pointer;
  display: none;
  align-items: center;
  justify-content: center;
  user-select: none;
  transition: background-color 0.3s;
}

.plus-button:hover {
  background-color: #e8e8e8;
}

.plus-button:active {
  background-color: #707070;
}

.board {
  min-width: 270px;
  margin: 10px;
  padding: 10px;
  min-height: 100px;
}

.board ul {
  list-style: none;
  padding: 0;
}

.board-title {
  user-select: none;
}
</style>
