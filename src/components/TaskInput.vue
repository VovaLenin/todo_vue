<template>
  <input
    :value="modelValue"
    @input="$emit('update:modelValue', $event.target.value)"
    ref="`item-input-${id}`"
    @keydown.enter="stopEditing"
    @blur="stopEditing"
  />
</template>

<script>
export default {
  props: {
    modelValue: {
      type: String,
    },
    id: {
      type: Number,
    },
  },
  methods: {
    stopEditing() {
      this.$emit("stop-editing");
    },
  },
};
</script>

<style scoped>
input {
  background: transparent;
  border: none;
  width: 100%;
  font-size: 17px;
  color: rgb(255, 251, 0);
  line-height: 1.6;
}
</style>
