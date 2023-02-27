<script setup>
import { ref, onMounted, watch, computed } from 'vue';

let id = 0;

const newTodo = ref('');
const hideCompleted = ref(false)
const todos = ref([
  { id: id++, text: 'Learn HTML', done: true },
  { id: id++, text: 'Learn JavaScript', done: true },
  { id: id++, text: 'Learn Vue', done: false }
]);

const filteredTodos = computed(() => {
  return hideCompleted.value ? todos.value.filter((t) => !t.done) : todos.value; 
})

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
    <form class="add-todo" @submit.prevent="addTodo">
      <h4>Add todo</h4>
      <input class="add-todo__input" type="text" v-model="newTodo" placeholder="e.g. Learn Vue" />
      <button class="add-todo__btn btn btn-add" title="Add Task">🚀</button>
      <button @click="hideCompleted = !hideCompleted" class="add-todo__btn btn btn-hide" :title="hideCompleted ? 'Show Completed' : 'Hide Completed'">{{ hideCompleted ? '👁' : '👁‍🗨'}}</button>
    </form>
    <ul>
      <div class="list">
        <li v-for="todo in filteredTodos" :key="todo.id" :class="`list__item ${todo.done}`">
          <input type="checkbox" v-model="todo.done" class="list__checkbox">
          <span class="list__text">{{ todo.text }}</span>
          <button @click="removeTodo(todo)" type="checkbox" class="btn btn-delete" title="Delete Task">✖</button>
        </li>
        <!-- <button @click="isChecked = !isChecked" class="btn">{{!isChecked ? 'Check all' : 'Uncheck All'}}</button> -->
      </div>
    </ul> 
    
  </div>
</template>
