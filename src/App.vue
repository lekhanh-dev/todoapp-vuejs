<template>
  <section id="app"></section>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      listTodo: [],
    };
  },
  created: function() {
    this.listTodo = localStorage.getItem("listTodo");
  },
  beforeMount: function() {
    console.log(this.listTodo);
  },
};
</script>

<style>
/* #app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
} */
</style>
