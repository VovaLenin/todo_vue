<template>
  <h1 class="page-title">Список дел</h1>
  <!-- <TodoList v-bind:todos="todos" @remove-todo="removeTodo" /> -->
  <ul class="board-container">
    <li
      class="board"
      v-for="board in boards"
      :key="board.id"
      @drop.prevent="dropBoard($event, board)"
      @dragover.prevent="dragOver($event)"
    >
      <h1 class="board-title">{{ board.title }}</h1>
      <hr />
      <ul>
        <li
          class="board-item"
          v-for="item in board.items"
          :key="item.id"
          :draggable="true"
          @dragover.prevent="dragOver($event)"
          @dragleave="dragLeave($event)"
          @dragstart="dragStart($event, board, item)"
          @dragend="dragEnd($event)"
          @drop.prevent="drop($event, board, item)"
        >
          {{ item.title }}
        </li>
      </ul>
    </li>
  </ul>
</template>

<script>
// import TodoList from "@/components/TodoList";
// export default {
//   name: "App",
//   data() {
//     return {
//       todos: [
//         {
//           id: 1,
//           title: "Встать раньше кота",
//           completed: false,
//         },
//         { id: 2, title: "Завидовать коту", completed: false },
//         { id: 3, title: "Разбудить кота", completed: false },
//         { id: 4, title: "Блаженно лечь спать", completed: false },
//       ],
//     };
//   },
//   components: {
//     TodoList,
//   },
//   methods: {
//     removeTodo(id) {
//       this.todos = this.todos.filter((todo) => todo.id !== id);
//     },
//   },
// };
export default {
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
    },
    dropBoard(event, board) {
      this.currentBoard.items = this.currentBoard.items.filter(
        (item) => item !== this.currentItem
      );
      board.items.push(this.currentItem);
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
  /* outline: 1px solid red; */
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
}
.board-container {
  list-style-type: none;
  display: flex;
  justify-content: space-around;
  height: 100%;
  flex-grow: 1;
  flex-wrap: wrap;
}
.board {
  min-width: 270px;
  margin: 10px;
  padding: 10px;
  min-height: 100px;
  .board-title {
    margin: 0;
    text-align: center;
  }
  ul {
    list-style: none;
    padding: 0;
  }
}
.board-item {
  padding: 5px;
  margin: 5px;
  background-color: rgba(0, 0, 255, 0.5);
  color: #fff;
  border-radius: 10px;
  cursor: grab;
  width: 239px;

  &:active {
    cursor: grabbing;
  }
}
</style>
