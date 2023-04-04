<template>
  <div id="root">
    <div class="todo-container">
      <div class="todo-wrap">
        <MyHeader @AddTodo="AddTodo" />
        <MyList :todos="todos" />
        <MyFooter
          :todos="todos"
          @CheckAllTodo="CheckAllTodo"
          @ClearDone="ClearDone"
        />
      </div>
    </div>
  </div>
</template>

<script>
// 引入组件
import MyHeader from "./components/MyHeader.vue";
import MyFooter from "./components/MyFooter.vue";
import MyList from "./components/MyList.vue";

export default {
  name: "App",
  components: { MyHeader, MyFooter, MyList },
  data() {
    return {
      todos: JSON.parse(localStorage.getItem("todos")) || [],
    };
  },
  methods: {
    AddTodo(e) {
      this.todos.unshift(e);
    },
    Check(id) {
      this.todos.forEach((todo) => {
        if (todo.id == id) todo.done = !todo.done;
      });
    },
    UpdateTodo(id,title) {
      this.todos.forEach((todo) => {
        if (todo.id == id) todo.name =title;
      });
    },
    DeleteTodo(id) {
      this.todos = this.todos.filter((todos) => todos.id != id);
    },
    CheckAllTodo(done) {
      this.todos.forEach((todos) => (todos.done = done));
    },
    ClearDone() {
      this.todos = this.todos.filter((todo) => !todo.done);
    },
  },
  watch: {
    todos: {
      deep: true,
      handler(value) {
        localStorage.setItem("todos", JSON.stringify(value));
      },
    },
  },
  mounted() {
    this.$bus.$on("Check", this.Check);
    this.$bus.$on("UpdateTodo", this.UpdateTodo);
    this.$bus.$on("DeleteTodo", this.DeleteTodo);
  },
  beforeDestroy() {
    this.$bus.$off('Check')
    this.$bus.$off("UpdateTodo")
    this.$bus.$off('DeleteTodo')
  },
};
</script>

<style >
/*base*/
body {
  background: #fff;
}

.btn {
  display: inline-block;
  padding: 4px 12px;
  margin-bottom: 0;
  font-size: 14px;
  line-height: 20px;
  text-align: center;
  vertical-align: middle;
  cursor: pointer;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2),
    0 1px 2px rgba(0, 0, 0, 0.05);
  border-radius: 4px;
}

.btn-danger {
  color: #fff;
  background-color: #da4f49;
  border: 1px solid #bd362f;
}

.btn-edit {
  color: #fff;
  background-color: skyblue;
  border: 1px solid rgb(65, 115, 134);
  margin-right: 5px;
}

.btn-danger:hover {
  color: #fff;
  background-color: #bd362f;
}

.btn:focus {
  outline: none;
}

.todo-container {
  width: 600px;
  margin: 0 auto;
}

.todo-container .todo-wrap {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}
</style>