<template>
  <li
    class="board-item"
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
    <TaskInput
      v-show="isEditingItem(item)"
      :ref="`item-${item.id}`"
      :item="item"
      v-model="item.title"
      @is-editing-item="isEditingItem(item)"
      @stop-editing="stopEditing()"
    />
    <button class="remove-button" @click="removeItem(item, board)">
      &times;
    </button>
  </li>
</template>

<script>
import TaskInput from "@/components/TaskInput";

export default {
  props: ["item", "board"],
  components: [TaskInput],
  data() {
    return {
      isEditingItem: false,
    };
  },
  methods: {
    editItem() {
      this.isEditingItem = true;
      this.$nextTick(() => {
        this.$refs[`item-input-${this.item.id}`].focus();
      });
    },
    stopEditing() {
      this.isEditingItem = false;
    },
  },
};
</script>

<style>
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
}

.board-item input {
  display: block;
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

.board-item:hover .remove-button {
  display: block;
}

.board-item:active {
  cursor: grabbing;
}
</style>
