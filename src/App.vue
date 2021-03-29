<template>
  <section id="app" class="container">
    <comp-header />
    <comp-control
      :textSearch="textSearch"
      @changeTextSearch="changeTextSearch"
    />
    <comp-form
      @add="addTodo"
      :isShowForm="isShowForm"
      @showForm="showForm"
      :todoSelected="todoSelected"
      @update="updateTodo"
    />
    <todo-table
      :listTodo="getListTodoBySearchText"
      @delete="deleteTodo"
      @edit="editTodo"
      @changeStatus="changeStatus"
      @changeAllStatus="changeAllStatus"
    />
  </section>
</template>

<script>
import TodoTable from "./components/TodoTable.vue";
import CompHeader from "./components/CompHeader.vue";
import CompControl from "./components/CompControl.vue";
import CompForm from "./components/CompForm.vue";

// import listTodo from "./mocks/todos";

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
      if (this.textSearch) {
        let todos = [];
        todos = this.listTodo.filter((todo) =>
          todo.content
            .toLowerCase()
            .includes(this.textSearch.trim().toLowerCase())
        );
        return todos;
      }
      return this.listTodo;
    },
  },
  methods: {
    handleSearch(value) {
      this.listTodo = this.listTodo.filter((todo) => todo.content.match(value));
      // console.log(this.listTodo);
    },
    addTodo(todo) {
      // console.log(todo);
      if (todo.content.trim()) {
        let todoObj = {
          content: todo.content,
          id: Math.floor(Math.random() * 100000000),
          completed: false,
          important: parseInt(todo.important),
        };
        this.listTodo.push(todoObj);
        this.updateData();
      }
    },
    showForm() {
      this.isShowForm = !this.isShowForm;
      if (!this.isShowForm) this.todoSelected = null;
    },
    deleteTodo(todoId) {
      this.listTodo = this.listTodo.filter((todo) => todoId !== todo.id);
      this.updateData();
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
      this.updateData();
    },
    changeStatus(id) {
      this.listTodo = this.listTodo.map((todo) => {
        if (todo.id === id) todo.completed = !todo.completed;
        return todo;
      });
      this.updateData();
    },
    changeTextSearch(text) {
      this.textSearch = text;
    },
    updateData() {
      let todosIsHigh = [];
      let todosIsMedium = [];
      let todosIsLow = [];
      this.listTodo.forEach((todo) => {
        if (todo.important === 2) todosIsHigh.push(todo);
        if (todo.important === 1) todosIsMedium.push(todo);
        if (todo.important === 0) todosIsLow.push(todo);
      });
      this.listTodo = todosIsHigh.concat(todosIsMedium, todosIsLow);
      localStorage.setItem("listTodo", JSON.stringify(this.listTodo));
    },
    changeAllStatus(checked) {
      this.listTodo = this.listTodo.map((todo) => {
        todo.completed = checked;
        return todo;
      });
      this.updateData();
    },
  },
  created: function() {
    let listTodo = localStorage.getItem("listTodo");
    if (listTodo) {
      this.listTodo = JSON.parse(localStorage.getItem("listTodo"));
    } else {
      localStorage.setItem("listTodo", JSON.stringify([]));
    }
  },
  // beforeUpdate() {
  //   console.log("before update");
  // },
  // beforeMount: function() {
  //   console.log("before mount");
  // },
};
</script>

<style>
:root {
  --blue: rgb(74, 122, 252);
  --white: #ffffff;
  --gray: rgb(241, 241, 241);

  font-size: 16px;
  background: var(--gray);
}
html {
  box-sizing: border-box;
  position: relative;
}

h1 {
  font-size: 3rem;
  color: var(--blue);
  text-align: center;
}

h2 {
  font-size: 1.5rem;
  color: var(--blue);
}
section section {
  background: var(--white);
  margin-bottom: 2.5rem;
  border-radius: 10px;
  padding: 1rem;
  box-shadow: 0px 0px 10px #d9d9d9;
}

button {
  border: none;
  outline: none;
  padding: 10px 20px;
  color: var(--white);
  background: var(--blue);
  border-radius: 3px;
  cursor: pointer;
}

input[type="text"],
select {
  outline: none;
  border-radius: 5px;
  padding: 10px;
  border: 1px solid #d9d9d9;
  margin-right: 20px;
}
input[type="checkbox"] {
  cursor: pointer;
}
select > option {
  width: 100px;
}
.d-flex {
  display: flex;
}
.flex-justify-c-center {
  justify-content: center;
}
.flex-align-i-center {
  align-items: center;
}
.flex-wrap {
  flex-wrap: wrap;
}
.container {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 30px;
  padding: 0 20px;
}
</style>
