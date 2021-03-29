<template>
  <section class="form">
    <button @click="showForm">{{ isShowForm ? "x" : "+" }}</button>
    <div v-show="isShowForm">
      <h2>{{ todoSelected ? "Update todo" : "Add new todo" }}</h2>
      <input type="text" placeholder="Enter name" v-model="content" />
      <select v-model="level">
        <option value="2">High</option>
        <option value="1">Medium</option>
        <option value="0">Low</option>
      </select>
      <button v-show="todoSelected" @click="updateTodo">Update</button>
      <button v-show="!todoSelected" @click="addTodo">Add</button>
    </div>
  </section>
</template>

<script>
export default {
  name: "comp-form",
  data() {
    return {
      content: "",
      level: "0",
    };
  },
  props: {
    isShowForm: Boolean,
    todoSelected: Object,
  },
  methods: {
    addTodo() {
      this.$emit("add", { content: this.content, important: this.level });
    },
    showForm() {
      this.$emit("showForm");
      this.resetData();
    },
    resetData() {
      this.content = "";
      this.level = 0;
    },
    updateTodo() {
      this.$emit("update", {
        content: this.content,
        important: this.level,
        id: this.todoSelected.id,
      });
    },
  },
  watch: {
    todoSelected: function(newData) {
      if (this.todoSelected) {
        this.content = newData.content;
        this.level = newData.important;
      }
    },
  },
};
</script>
