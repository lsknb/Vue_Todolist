<template>
  <li>
    <label>
      <input
        type="checkbox"
        :checked="todoObj.done"
        @change="Change(todoObj.id)"
      />
      <span v-show="!todoObj.isEdit">{{ todoObj.name }}</span>
      <input
        v-show="todoObj.isEdit"
        type="text"
        :value="todoObj.name"
        @blur="HandleBlur(todoObj,$event)"
        ref="inputTitle"
      />
    </label>
    <button class="btn btn-danger" @click="HandDelete(todoObj.id)">删除</button>
    <button class="btn btn-edit" v-show="!todoObj.isEdit" @click="HandleEdit(todoObj)">编辑</button>
  </li>
</template>

<script>
export default {
  name: "MyItem",
  props: ["todoObj"],
  methods: {
    Change(id) {
      //   this.Check(id);
      this.$bus.$emit("Check", id);
    },
    HandDelete(id) {
      if (confirm("确认删除吗？")) {
        // this.DeleteTodo(id)
        this.$bus.$emit("DeleteTodo", id);
      }
    },
    HandleEdit(todo) {
      if (todo.hasOwnProperty("isEdit")) {
        todo.isEdit = true;
      } else {
        this.$set(todo, "isEdit", true);
      }
      this.$nextTick(function(){
        this.$refs.inputTitle.focus()
      })
    },
    HandleBlur(todo,e) {
      todo.isEdit = false;
      if(!e.target.value.trim())return alert('输入不能为空！')
      this.$bus.$emit('UpdateTodo',todo.id,e.target.value)
    },
  },
};
</script>

<style scoped>
/*item*/
li {
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ddd;
}

li label {
  float: left;
  cursor: pointer;
}

li label li input {
  vertical-align: middle;
  margin-right: 6px;
  position: relative;
  top: -1px;
}

li button {
  float: right;
  display: none;
  margin-top: 3px;
}

li:before {
  content: initial;
}

li:last-child {
  border-bottom: none;
}

li:hover {
  background-color: #ddd;
}
li:hover button {
  display: block;
}
</style>