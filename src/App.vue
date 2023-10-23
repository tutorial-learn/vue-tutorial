<script lang="ts">
import HelloWorld from "./components/HelloWorld.vue";

export default {
  components: {
    HelloWorld,
  },
  data() {
    return {
      msg: "hello world",
    };
  },
};
</script>

<template>
  <HelloWorld :msg="msg" />
</template>

<style scoped></style>
