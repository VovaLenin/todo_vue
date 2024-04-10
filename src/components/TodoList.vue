<template>
  <div>
    <ul>
      <TodoItem
        v-for="todo in todos"
        :key="todo.id"
        :todo="todo"
        @remove-todo="removeTodo"
      />
    </ul>
  </div>
</template>

<script>
import TodoItem from "@/components/TodoItem";
export default {
  props: ["todos"],
  components: { TodoItem },
  methods: {
    removeTodo(id) {
      this.$emit("remove-todo", id);
    },
  },
};
</script>

<style scoped>
ul {
  list-style: none;
}
</style>
