<template>
  <todo-form @x-add="addTodo"></todo-form>

  <button type="button" @click="toggleIsDone">
    <span v-if="!isDone">미완료 리스트만 보이기</span>
    <span v-else>모든 리스트 보이기</span>
  </button>

  <ul>
    <li v-for="todo in computedTodoList" :key="todo.id">
      <input
        type="checkbox"
        :checked="todo.done"
        @input="doneTodo(todo.id, $event)"
      />
      <span>{{ todo.text }}</span>
      <button type="button" @click="removeTodo(todo.id)">❌</button>
    </li>
  </ul>
</template>

<script lang="ts">
import { v4 as uuidV4 } from "uuid";
import { localStorageKey } from "@/constants/storageKey";
import TodoForm from "./components/TodoForm.vue";

interface IState {
  todoList: { id: string; text: string; done: boolean }[];
  isDone: boolean;
}

export default {
  components: {
    "todo-form": TodoForm,
  },
  data(): IState {
    return {
      todoList: [],
      isDone: false,
    };
  },
  computed: {
    computedTodoList() {
      return this.isDone
        ? this.todoList.filter((todo) => !todo.done)
        : this.todoList;
    },
  },
  mounted() {
    const exist = localStorage.getItem(localStorageKey.TODO_LIST);
    if (exist) this.todoList = JSON.parse(exist);
  },
  methods: {
    $_private_localstorage() {
      localStorage.setItem(
        localStorageKey.TODO_LIST,
        JSON.stringify(this.todoList)
      );
    },
    addTodo(newText: string) {
      this.todoList.push({ id: uuidV4(), text: newText, done: false });
      this.$_private_localstorage();
    },
    removeTodo(id: string) {
      this.todoList = this.todoList.filter((todo) => todo.id !== id);
      this.$_private_localstorage();
    },
    doneTodo(id: string, e: Event) {
      this.todoList = this.todoList.map((todo) =>
        todo.id === id
          ? { ...todo, done: (<HTMLInputElement>e.target).checked }
          : todo
      );
      this.$_private_localstorage();
    },
    toggleIsDone() {
      this.isDone = !this.isDone;
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
