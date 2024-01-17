<template>
  <form @submit.prevent="addNewTodo">
    <input type="text" :value="text" @input="(e) => (text = e.target.value)" />
    <button>Add Todo</button>
  </form>
  <ul>
    <li v-for="todo in router" :key="todo.id">
      <input
        type="checkbox"
        :value="todo.done"
        @change="todo.done = !todo.done"
        :checked="todo.done"
      />
      <span :class="{ done: todo.done }">{{ todo.text }}{{ " " }}</span>
      <button @click="removeTodo(todo)">X</button>
    </li>
  </ul>
  <button
    @click="
      filteredTodo.length !== todos.length && (hideCompleted = !hideCompleted)
    "
  >
    {{ hideCompleted ? "Show all" : "Hide completed" }}
  </button>
</template>
<script setup>
import { ref, computed } from "vue";
let id = 0;
const text = ref("");
const hideCompleted = ref(false);
const createTodo = (text) => ({ id: id++, text, done: false });
const todos = ref([createTodo("Learn JavaScript"), createTodo("Learn Vue")]);
const filteredTodo = computed(() => todos.value.filter((t) => !t.done));
const router = computed(() =>
  hideCompleted.value ? filteredTodo.value : todos.value,
);
const addNewTodo = () => {
  todos.value.push(createTodo(text.value));
  text.value = "";
};
const removeTodo = (todo) => {
  todos.value = todos.value.filter((t) => t !== todo);
};
</script>
<style scoped>
.done {
  text-decoration: line-through;
}
</style>
