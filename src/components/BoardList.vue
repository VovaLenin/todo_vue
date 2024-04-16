<template>
  <ul class="board-container">
    <BoardItem
      v-for="board in boards"
      :board="board"
      :key="board.id"
      @edit-item="editItem"
      @stop-editing="stopEditing"
      :modelValue="modelValue"
      @update:modelValue="$emit('update:modelValue', $event)"
      @is-editing-item="isEditingItem"
      @remove-item="removeItem"
      @add-item="addItem"
      @drag-start="dragStart"
      @drag-end="dragEnd"
      @drag-over="dragOver"
      @drag-leave="dragLeave"
      @drop="drop"
      @drop-board="dropBoard"
    />
  </ul>
</template>

<script>
import BoardItem from "./BoardItem.vue";

export default {
  components: {
    BoardItem,
  },
  props: ["boards", "modelValue"],
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

<style scoped>
.board-container {
  list-style-type: none;
  display: flex;
  justify-content: space-around;
  height: 100%;
  flex-grow: 1;
  flex-wrap: wrap;
}
</style>
