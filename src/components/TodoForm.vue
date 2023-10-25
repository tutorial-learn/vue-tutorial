<template>
  <form @submit.prevent="onSubmit">
    <input v-model="inputText" />
    <button type="submit">Submit</button>
  </form>
</template>

<script lang="ts">
export default {
  data() {
    return {
      inputText: "",
    };
  },
  methods: {
    onSubmit() {
      this.$emit("x-add", this.inputText);
      this.inputText = "";
    },
  },
};
</script>
