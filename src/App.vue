<template>
  <section id="app" class="container">
    <comp-header />
    <comp-control
      :text-search="textSearch"
      @changeTextSearch="changeTextSearch"
    />
    <comp-form
      @add="addTodo"
      :is-show-form="isShowForm"
      @showForm="showForm"
      :todo-selected="todoSelected"
      @update="updateTodo"
    />
    <todo-table
      :list-todo="getListTodoBySearchText"
      @delete="deleteTodo"
      @edit="editTodo"
      @changeStatus="changeStatus"
      @changeAllStatus="changeAllStatus"
    />
  </section>
</template>

<script>
import "./style.css";
import TodoTable from "./components/TodoTable.vue";
import CompHeader from "./components/CompHeader.vue";
import CompControl from "./components/CompControl.vue";
import CompForm from "./components/CompForm.vue";

export default {
  name: "App",
  data() {
    return {
      listTodo: [],
      isShowForm: false,
      todoSelected: null,
      textSearch: "",
    };
  },
  components: {
    CompHeader,
    CompControl,
    TodoTable,
    CompForm,
  },
  computed: {
    getListTodoBySearchText() {
      return this.listTodo
        .filter((todo) =>
          todo.content
            .toLowerCase()
            .includes(this.textSearch.trim().toLowerCase())
        )
        .sort((a, b) => b.important - a.important);
    },
  },
  methods: {
    handleSearch(value) {
      this.listTodo = this.listTodo.filter((todo) => todo.content.match(value));
    },
    addTodo(todo) {
      if (todo.content.trim()) {
        let todoObj = {
          content: todo.content,
          id: Math.floor(Math.random() * 100000000),
          completed: false,
          important: parseInt(todo.important),
        };
        this.listTodo.push(todoObj);
        this.saveInLocalStorage();
      }
    },
    showForm() {
      this.isShowForm = !this.isShowForm;
      if (!this.isShowForm) this.todoSelected = null;
    },
    deleteTodo(todoId) {
      this.listTodo = this.listTodo.filter((todo) => todoId !== todo.id);
      this.saveInLocalStorage();
    },
    editTodo(todo) {
      this.todoSelected = todo;
      this.isShowForm = true;
    },
    updateTodo(todoObj) {
      this.listTodo = this.listTodo.map((todo) => {
        if (todo.id === todoObj.id) {
          todo.content = todoObj.content;
          todo.important = todoObj.important;
        }
        return todo;
      });
      this.todoSelected = null;
      this.isShowForm = false;
      this.saveInLocalStorage();
    },
    changeStatus(id) {
      this.listTodo = this.listTodo.map((todo) => {
        if (todo.id === id) todo.completed = !todo.completed;
        return todo;
      });
      this.saveInLocalStorage();
    },
    changeTextSearch(text) {
      this.textSearch = text;
    },
    saveInLocalStorage() {
      localStorage.setItem("listTodo", JSON.stringify(this.listTodo));
    },
    changeAllStatus(checked) {
      this.listTodo = this.listTodo.map((todo) => {
        todo.completed = checked;
        return todo;
      });
      this.saveInLocalStorage();
    },
  },
  created: function() {
    let listTodo = localStorage.getItem("listTodo");
    if (listTodo) {
      this.listTodo = JSON.parse(listTodo);
    } else {
      localStorage.setItem("listTodo", JSON.stringify([]));
    }
  },
};
</script>

<style></style>
