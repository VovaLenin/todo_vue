<template>
  <h1 class="page-title">Список дел</h1>
  <BoardList />
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
    dragStart(event, board, item) {
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
    drop(event, board, item) {
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
    dropBoard(event, board) {
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
      const parentRef = this.$refs[`item-${item.id}`].target;
      console.log(parentRef);
      if (parentRef) {
        this.$nextTick(() => {
          const inputRef = parentRef.$refs[`item-input-${item.id}`];
          if (inputRef) {
            // Делаем что-то с инпутом
            // Например, устанавливаем фокус
            inputRef.focus();
          }
        });
      }
    },
    stopEditing() {
      this.editingItem = null;
    },
    removeItem(item, board) {
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
