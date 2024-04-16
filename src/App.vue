<template>
  <h1 class="page-title">Список дел</h1>
  <BoardList
    :boards="boards"
    @update:modelValue="updateTitle($event)"
    @edit-item="editItem"
    @stop-editing="stopEditing"
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
</template>

<script>
import BoardList from "@/components/BoardList";

export default {
  components: {
    BoardList,
  },
  data() {
    return {
      boards: [
        {
          id: 1,
          title: "Сделать",
          items: [
            { id: 1, title: "Проснуться раньше кота" },
            { id: 2, title: "Завидовать коту" },
            { id: 3, title: "Разбудить кота" },
            { id: 4, title: "Блаженно лечь спать" },
          ],
        },
        {
          id: 2,
          title: "В работе",
          items: [],
        },
        {
          id: 3,
          title: "Готово",
          items: [],
        },
      ],
      currentBoard: null,
      currentItem: null,
      lastDraggedOverElement: null,
      newItemTitle: "",
      editingItem: null,
    };
  },
  methods: {
    dragStart({ board, item }) {
      this.currentBoard = board;
      this.currentItem = item;
    },
    dragEnd(event) {
      event.target.style.boxShadow = "none";
      if (this.lastDraggedOverElement) {
        this.lastDraggedOverElement.style.boxShadow = "none";
        this.lastDraggedOverElement = null;
      }
    },
    dragOver(event) {
      if (event.target.classList.contains("board-item")) {
        event.target.style.boxShadow = "0 4px 3px gray";
        this.lastDraggedOverElement = event.target;
      } else {
        if (this.lastDraggedOverElement) {
          this.lastDraggedOverElement.style.boxShadow = "none";
          this.lastDraggedOverElement = null;
        }
      }
    },
    dragLeave(event) {
      event.target.style.boxShadow = "none";
      if (event.target === this.lastDraggedOverElement) {
        event.target.style.boxShadow = "none";
        this.lastDraggedOverElement = null;
      }
    },
    drop({ event, board, item }) {
      if (this.currentItem) {
        event.stopPropagation();
        this.currentBoard.items = this.currentBoard.items.filter(
          (item) => item !== this.currentItem
        );
        const dropIndex = board.items.indexOf(item);
        board.items.splice(dropIndex + 1, 0, this.currentItem);
        this.boards = this.boards.map((b) => {
          if (b.id === board.id) {
            return board;
          }
          if (b.id === this.currentBoard.id) {
            return this.currentBoard;
          }
          return b;
        });
        this.currentItem = null;
      }
    },
    dropBoard(board) {
      if (this.currentItem) {
        this.currentBoard.items = this.currentBoard.items.filter(
          (item) => item !== this.currentItem
        );
        board.items.push(this.currentItem);
        this.currentItem = null;
      }
    },
    isEditingItem(item) {
      return this.editingItem === item;
    },
    editItem(item) {
      this.editingItem = item;
    },
    stopEditing() {
      this.editingItem = null;
    },
    removeItem({ item, board }) {
      console.log(board);
      const index = board.items.indexOf(item);
      if (index !== -1) {
        board.items.splice(index, 1);
      }
      if (this.isEditingItem(item)) {
        this.stopEditing();
      }
    },
    addItem(board) {
      const newTitle = prompt("Новая задача:");
      if (newTitle) {
        const newItem = {
          id: Date.now(),
          title: newTitle,
        };
        board.items.push(newItem);
      }
    },
    updateTitle(event) {
      console.log(event);
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Open+Sans:wght@400;600;700&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  font-family: "Open Sans", sans-serif;
  background-color: #f5f5f5;
  color: #333;
  line-height: 1.6;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

.page-title {
  text-align: center;
  user-select: none;
}
</style>
