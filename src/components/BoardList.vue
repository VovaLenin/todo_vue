<template>
  <ul class="board-container">
    <BoardItem v-for="board in boards" :key="board.id" />
  </ul>
</template>

<script>
import BoardItem from "./BoardItem.vue";

export default {
  components: {
    BoardItem,
  },
  props: ["boards"],
  methods: {},
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