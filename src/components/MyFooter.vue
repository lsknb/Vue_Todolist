<template>
    <div class="todo-footer" v-show="this.todos.length">
      <label>
        <input type="checkbox" :Checked="Checked" @change="CheckAll"/>
      </label>
      <span> <span>已完成{{ TotalTodo }}</span> / 全部{{todos.length}} </span>
      <button class="btn btn-danger" @click="ClearAllDone">清除已完成任务</button>
    </div>
</template>

<script>
export default {
  name: "MyFooter",
  props:['todos'],
  computed:{
    TotalTodo(){
        return this.todos.reduce((pre,todo)=>pre+ (todo.done? 1 :0),0)
    },
    Checked(){
        return this.todos.length===this.TotalTodo&&this.todos.length>0
    }
  },
  methods:{
    CheckAll(e){
        //this.CheckAllTodo(e.target.checked)
        this.$emit('CheckAllTodo',e.target.checked)
       // console.log(e.target.checked);
    },
    ClearAllDone(){
        //this.ClearDone()
        this.$emit('ClearDone')
    }
  }
};
</script>

<style scoped>
/*footer*/
.todo-footer {
  height: 40px;
  line-height: 40px;
  padding-left: 6px;
  margin-top: 5px;
}

.todo-footer label {
  display: inline-block;
  margin-right: 20px;
  cursor: pointer;
}

.todo-footer label input {
  position: relative;
  top: -1px;
  vertical-align: middle;
  margin-right: 5px;
}

.todo-footer button {
  float: right;
  margin-top: 5px;
}
</style>