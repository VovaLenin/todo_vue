<template>
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
