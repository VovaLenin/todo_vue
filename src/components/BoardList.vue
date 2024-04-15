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
