<template>
  <todo-form @x-add="addTodo" />

  <ul>
    <li v-for="todo in todoList" :key="todo.id">
      <span>{{ todo.text }}</span>
      <button type="button" @click="removeTodo(todo.id)">❌</button>
    </li>
  </ul>
</template>

<script lang="ts">
import { v4 as uuidV4 } from "uuid";

import TodoForm from "./components/TodoForm.vue";

interface IState {
  todoList: { id: string; text: string }[];
}

export default {
  components: {
    "todo-form": TodoForm,
  },
  data(): IState {
    return {
      todoList: [],
    };
  },
  methods: {
    addTodo(newText: string) {
      this.todoList.push({ id: uuidV4(), text: newText });
    },
    removeTodo(id: string) {
      this.todoList = this.todoList.filter((todo) => todo.id !== id);
    },
  },
};
</script>

<style scoped>
ul {
  margin-top: 20px;
}

li {
  display: flex;
  gap: 10px;
  margin: 5px 0;
}
</style>
