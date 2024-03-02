<template>
  <div>
    <button @click="increment">Increment</button>
    <button @click="decrement">Decrement</button>
    <button @click="addTodo">AddTodo</button>
    <button @click="sortAscTodos">Sort Asc</button>
    <button @click="sortDescTodos">Sort Desc</button>
    <transition name="fade">
      <p v-if="count >= 3" style="color: red">{{ count }}</p>
      <p v-else style="color: green">{{ count }}</p>
    </transition>
    <div class="container">
      <div class="list" v-for="todo in todos" :key="todo.completed">
        <div class="item">
          <input type="checkbox" v-model="todo.completed" />
          <span>{{ todo.title }}</span
          ><button @click="() => removeTodo(todo.id)">Remove</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="js">
import { onMounted, ref } from 'vue';

interface ITodo {
  userId: number
  id: number
  title: string
  completed: boolean
}

const count = ref(0);
const todos = ref<ITodo[]>()

function increment() {
  count.value++
}

function decrement() {
  count.value--
}

function loadTodos(){
  fetch('https://jsonplaceholder.typicode.com/todos/')
      .then(response => response.json())
      .then((res) => {
        todos.value = res.slice(0,10)
      })
}
function addTodo(){
  todos.value.push({userId: 1, id: Math.random(), title: 'lol', completed: false})
}

function removeTodo(id: number){
  todos.value = todos.value.filter(t => t.id !== id)
}

function sortAscTodos(){
  todos.value = todos.value.sort((a, b) => a.id + b.id);
}

function sortDescTodos(){
  todos.value = todos.value.sort((a, b) => b.id + a.id);
}

onMounted(() => {
  loadTodos()
})
</script>

<style>
.list {
  display: flex;
  flex-direction: column;
}

.item {
  display: flex;
  gap: 10px;
  margin: 2px;
  justify-content: space-between;
  padding: 5px 10px;
  border-radius: 4px;
  border: 1px solid lightgray;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.1s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
