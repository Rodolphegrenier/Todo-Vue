<script setup>
import TodoCreator from '@/components/TodoCreator.vue';
import {ref, watch, computed} from "vue";
import {uid} from 'uid';
import TodoItem from '@/components/TodoItem.vue';
import { Icon } from "@iconify/vue";
const todoList = ref([]);

watch(
  todoList,
  () => {
    setTodoListLocalStorage();
  }, {
  deep: true,
});

const todoCompleted = computed(() => {
  return todoList.value.every((todo) => todo.isCompleted)
})

const fetchTodoList = () => {
  const savedTodoList = JSON.parse(localStorage.getItem("todoList"));
  if (savedTodoList) {
    todoList.value = savedTodoList;
  }
};

fetchTodoList();

const setTodoListLocalStorage = () => {
  localStorage.setItem("todoList", JSON.stringify(todoList.value));
  
};



const createTodo = (todo, deadLine) => {
  todoList.value.push({
  id: uid(),
  todo,
  deadLine,
  isCompleted: null,
  isEditing: null,
  })
  todoList.value.sort((a, b) => new Date(a.deadLine) - new Date(b.deadLine));
};

const toggleTodoComplete = (todoPos) => {
  todoList.value[todoPos].isCompleted = !todoList.value[todoPos].isCompleted;
};

const toggleEditTodo = (todoPos) => {
  todoList.value[todoPos].isEditing = !todoList.value[todoPos].isEditing;
  todoList.value.sort((a, b) => new Date(a.deadLine) - new Date(b.deadLine));
// setTodoListLocalStorage(); 
};

const updateTodo = (todoVal, todoDeadLine, todoPos) => {
  //  console.log(`Event received: field=${field}, value=${value}, index=${todoPos}`);
  todoList.value[todoPos].todo = todoVal;
  todoList.value[todoPos].deadLine = todoDeadLine;
  
  // setTodoListLocalStorage(); 
};

const deleteTodo = (todoId) => {
  todoList.value = todoList.value.filter((todo) => todo.id !== todoId);
  // setTodoListLocalStorage(); 
};

</script>

<template>
  <main>
    <h1>Create Todo</h1>
    <TodoCreator @create-todo="createTodo"/>
    <ul class="todo-list" v-if="todoList.length > 0">
      <TodoItem v-for="(todo, index) in todoList" :todo="todo" :index="index" @toggle-complete="toggleTodoComplete" @edit-todo="toggleEditTodo" @update-todo ="updateTodo" @delete-todo="deleteTodo"/>
    </ul>
    <p class="todos-msg" v-else>
      <Icon icon="garden:face-very-happy-stroke-16" width="22px"/>      <span> All tasks completed!</span>
    </p>
    <p v-if="todoCompleted && todoList.length >0" class="todos-msg">
      <Icon icon="noto-v1:party-popper"/>
    </p>
  </main>
</template>

<style lang="scss" scoped>
main {
  display: flex;
  flex-direction: column;
  max-width: 500px;
  width: 100%;
  margin: 0 auto;
  padding: 40px 16px;

  h1 {
    margin-bottom: 16px;
    text-align: center;
  }
  .todo-list {
    display: flex;
    flex-direction: column;
    list-style: none;
    margin-top: 24px;
    gap: 20px;
  }

  .todos-msg {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    margin-top: 24px;
  }
}
</style>