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
      <div class="title-container">
        <h1 class="board-title">{{ board.title }}</h1>
        <button class="plus-button" @click="addItem(board)">+</button>
      </div>
      <hr />
      <p v-if="board.items.length === 0">Нет активных задач</p>
      <ul>
        <li
          class="board-item"
          v-for="item in board.items"
          :key="item.id"
          :draggable="!isEditingItem(item)"
          @dblclick="editItem(item, board)"
          @dragover.prevent="dragOver($event)"
          @dragleave="dragLeave($event)"
          @dragstart="dragStart($event, board, item)"
          @dragend="dragEnd($event)"
          @drop.prevent="drop($event, board, item)"
        >
          <span v-if="!isEditingItem(item)">
            {{ item.title }}
          </span>
          <input
            v-show="isEditingItem(item)"
            v-model="item.title"
            :ref="`item-input-${item.id}`"
            @keydown.enter="stopEditing"
            @blur="stopEditing"
          />
          <button class="remove-button" @click="removeItem(item, board)">
            &times;
          </button>
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
      this.$nextTick(() => {
        const input = this.$refs[`item-input-${item.id}`];
        console.log(input);
        if (input.length !== 0) {
          input[0].focus();
        }
      });
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
  user-select: none;
}

.board-container {
  list-style-type: none;
  display: flex;
  justify-content: space-around;
  height: 100%;
  flex-grow: 1;
  flex-wrap: wrap;
}

.title-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  &:hover .plus-button {
    display: inline-flex;
  }
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
  &:hover {
    background-color: #e8e8e8;
  }
  &:active {
    background-color: #707070;
  }
}

.board {
  min-width: 270px;
  margin: 10px;
  padding: 10px;
  min-height: 100px;
  ul {
    list-style: none;
    padding: 0;
  }
}

.board-title {
  user-select: none;
}

.board-item {
  padding: 5px;
  margin: 5px;
  background-color: rgba(0, 0, 255, 0.5);
  color: #fff;
  border-radius: 10px;
  cursor: grab;
  width: 239px;
  min-height: 36px;
  display: flex;
  justify-content: space-between;

  & input {
    background: transparent;
    border: none;
    width: 100%;
    font-size: 17px;
    color: rgb(255, 251, 0);
    line-height: 1.6;
  }

  .remove-button {
    display: none;
    width: 25px;
    height: 25px;
    background: transparent;
    border: none;
    cursor: pointer;
    font-size: 25px;
    color: red;
  }

  &:hover .remove-button {
    display: block;
  }

  &:active {
    cursor: grabbing;
  }
}
</style>
