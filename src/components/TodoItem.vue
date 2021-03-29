<template>
  <div class="todos__item d-flex">
    <div class="todos__item-completed flex-col-1 d-flex flex-align-i-center">
      <input type="checkbox" :checked="todo.completed" @click="changeStatus" />
    </div>
    <div class="todos__item-name d-flex flex-col-7 flex-align-i-center">
      {{ todo.content }}
    </div>
    <div
      class="todos__item-important flex-col-2 d-flex flex-align-i-center flex-justify-c-center"
    >
      <span :class="getImportantClass">{{ getImportantName }}</span>
    </div>
    <div
      class="todos__item-active flex-col-2 d-flex flex-justify-c-center flex-align-i-center flex-wrap"
    >
      <button @click="editTodo">Edit</button
      ><button @click="deleteTodo">Delete</button>
    </div>
  </div>
</template>

<script>
import mapLevel from "../mocks/level";

export default {
  name: "todo-item",
  data() {
    return {};
  },
  props: {
    todo: Object,
  },
  methods: {
    deleteTodo() {
      let comfirm = confirm(
        "You have sure want delete todo: " + this.todo.content
      );
      if (comfirm) {
        this.$emit("delete", this.todo.id);
      }
    },
    editTodo() {
      this.$emit("edit", this.todo);
    },
    changeStatus() {
      this.$emit("changeStatus", this.todo.id);
    },
  },
  computed: {
    getImportantName() {
      return mapLevel[this.todo.important].name;
    },
    getImportantClass() {
      return `todos--${mapLevel[this.todo.important].className}`;
    },
  },
};
</script>

<style scoped>
span {
  padding: 5px;
  border-radius: 5px;
}
button {
  margin: 5px 0;
}
button:nth-child(1) {
  margin-right: 5px;
}
.todos__item:nth-child(2n + 1) {
  background: #f7f7f7;
}
.todos__item-important {
  color: var(--white);
}
.todos--high {
  background: rgb(255, 36, 36);
}
.todos--medium {
  background: greenyellow;
}
.todos--low {
  background: rgb(77, 77, 255);
}
.todos__item {
  padding: 20px;
}
</style>
