<template>
  <section class="todos">
    <h2>List todo</h2>
    <div class="todos__header d-flex">
      <div class="todos__header-completed flex-col-1">
        <input
          type="checkbox"
          :checked="checked"
          @click="updateAllStatus($event.target.checked)"
        />
      </div>
      <div class="todos__header-name flex-col-7">Name</div>
      <div
        class="todos__header-important flex-col-2 d-flex flex-justify-c-center flex-align-i-center"
      >
        Important
      </div>
      <div
        class="todos__header-action flex-col-2 d-flex flex-justify-c-center flex-align-i-center"
      >
        Action
      </div>
    </div>
    <div class="todos__list">
      <todo-item
        v-for="item in listTodo"
        :key="item.id"
        :todo="item"
        @edit="editTodo"
        @delete="deleteTodo"
        @changeStatus="changeStatus"
      />
    </div>
  </section>
</template>

<script>
import TodoItem from "./TodoItem.vue";
export default {
  name: "todo-table",
  props: {
    listTodo: Array,
  },
  components: {
    TodoItem,
  },
  methods: {
    deleteTodo(todoId) {
      this.$emit("delete", todoId);
    },
    editTodo(todo) {
      this.$emit("edit", todo);
    },
    changeStatus(id) {
      this.$emit("changeStatus", id);
    },
    updateAllStatus(checked) {
      this.$emit("changeAllStatus", checked);
    },
  },
  computed: {
    checked() {
      let check = this.listTodo.length !== 0;
      this.listTodo.forEach((todo) => {
        if (!todo.completed) {
          check = false;
        }
      });
      return check;
    },
  },
  mounted: function() {
    // console.log(this.listTodo);
  },
};
</script>

<style>
.todos h2 {
  padding: 0 20px;
}
.d-flex {
  display: flex;
}
.flex-col-1 {
  flex: 8.33%;
}
.flex-col-7 {
  flex: 58.33%;
}
.flex-col-2 {
  flex: 16.66%;
}
.todos__header {
  padding: 20px;
}
.todos {
  padding: 20px 0 0 0;
}
</style>
