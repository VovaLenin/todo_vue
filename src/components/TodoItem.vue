<template>
  <li>
    <div class="todo-item" :class="{ done: isActive }">
      <p>{{ todo.title }}</p>
      <div>
        <button></button>
        <button @click="$emit('remove-todo', todo.id)">&#10006;</button>
      </div>
    </div>
  </li>
</template>

<script>
export default {
  props: {
    todo: { type: Object, required: true },
  },
  data() {
    return {
      isActive: false,
    };
  },
  methods: {
    toggleClass() {
      this.isActive = !this.isActive;
    },
  },
};
</script>

<style scoped>
.todo-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 200px;
  height: 66px;
  border: 1px solid #eee;
  box-shadow: 3px 3px 7px rgba(198, 198, 198, 0.36);
  border-radius: 20px;
  background: #f7f6f7;
  padding: 0.5rem 0.8rem;
  color: #828282;
  cursor: grab;
  margin-bottom: 10px;
}

.todo-item:active {
  cursor: grabbing;
}

button {
  height: 20px;
  width: 20px;
  text-align: center;
  padding: 0;
  cursor: pointer;
  border-radius: 50%;
}

.done {
  background: black;
}
</style>
