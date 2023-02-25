<script setup>
import { ref, onMounted,watch } from 'vue';

let id = 0;

const newTodo = ref('');
const todos = ref([
]);

function addTodo() {
  if (newTodo.value.trim() == '' || newTodo.value === null) return 
  todos.value.push({ 
    id: id++, 
    text: newTodo.value, 
    done: false 
  });
  newTodo.value = ''
}

function removeTodo(todo) {
  todos.value = todos.value.filter((el) => el !== todo);
}


watch(todos, (newValue) => {
  localStorage.setItem('todos', JSON.stringify(newValue))
}, {deep: true})

onMounted(() => {
	todos.value = JSON.parse(localStorage.getItem('todos')) || []
})
</script>

<template>
  <div class="wrapper">
    <div class="header">
      <h1 class="header__title">Tasks</h1>
    </div>
    <form @submit.prevent="addTodo">
      <h4>Add todo</h4>
      <input type="text" v-model="newTodo" placeholder="e.g. Learn Vue" />
      <button class="btn btn-add">🚀</button>
    </form>
    <ul>
      <div class="list">
        <li v-for="todo in todos" :key="todo.id" :class="`list__item ${todo.done}`">
          <input type="checkbox" v-model="todo.done" class="list__checkbox">
          <span class="list__text">{{ todo.text }}</span>
          <button @click="removeTodo(todo)" type="checkbox" class="btn btn-delete" >✖</button>
        </li>
        <!-- <button @click="isChecked = !isChecked" class="btn">{{!isChecked ? 'Check all' : 'Uncheck All'}}</button> -->
      </div>
    </ul>
    
  </div>
</template>
