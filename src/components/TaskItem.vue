<template>
  <li
    class="board-item"
    :draggable="!isEditingItem"
    @dblclick="editItem"
    @dragover.prevent="dragOver"
    @dragleave="dragLeave"
    @dragstart="startDragging"
    @dragend="stopDragging"
    @drop.prevent="drop"
  >
    <span v-if="!isEditingItem">{{ item.title }}</span>
    <input
      v-show="isEditingItem"
      v-model="item.title"
      :ref="`item-input-${item.id}`"
      @keydown.enter="stopEditing"
      @blur="stopEditing"
    />
    <button class="remove-button" @click="$emit('removeItem')">&times;</button>
  </li>
</template>

<script>
export default {
  props: ['item', 'board'],
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
    dragStart() /* ... */,
    dragS() /* ... */,
    dragOver() /* ... */,
    dragLeave() /* ... */,
    drop() /* ... */,
  },
};
</script>
<!-- стили... -->
