<script setup>
import { reactive, ref, defineEmits } from 'vue';
import TodoButton from "./TodoButton.vue";
const emit = defineEmits(["create-todo"]);


const todoState = reactive(
  {
    todo: "",
    deadLine:null,
    invalid:null,
    errMsg: "",
  }
);

const createTodo = () => {
  todoState.invalid = null;
  if(todoState.todo !==""&& todoState.deadLine !== null) 
  {
    emit("create-todo", todoState.todo, todoState.deadLine);
    todoState.todo = "";
    todoState.deadLine = null;
  return;
  }
  todoState.invalid=true;
  todoState.errMsg="Todo value or date cannot be empty"
};

  
 
  
</script>

<template>
  <h1>Hellow</h1>
    <div class="input-wrap" :class="{'input-err' : todoState.invalid}">
            <input type="text" v-model = "todoState.todo">
            <input type="date" v-model="todoState.deadLine">
            <TodoButton @click="createTodo()"/>
    </div>
    <p v-if= "todoState.invalid" class="err-msg">{{ todoState.errMsg}}</p>
    <!-- <p v-show="todoState.invalid" class="err-msg">{{ todoState.errMsg}}</p> -->
</template>

<style lang="scss" scoped>
  .input-wrap {
  display: flex;
  // gap: 10px;
  transition: 250ms ease;
  border: 2px solid #41b080;

  &.input-err {
    border-color: red;
  }

  &:focus-within {
    box-shadow: 0 -4px 6px -1px rgb(0 0 0 / 0.1),
      0 -2px 4px -2px rgb(0 0 0 / 0.1);
  }

  

  input {
    flex: 1;
    // width: 100%;
    padding: 8px 6px;
    border: none;
    border: 2px solid #41b080;

    &:focus {
      outline: none;
    }
  }


  .err-msg {
  margin-top: 6px;
  font-size: 12px;
  text-align: center;
  color: red;
}

  }
</style>