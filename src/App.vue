<script setup>
import { ref, watch } from 'vue';
import { useTodoStore } from './stores/useTodoStore';
import { storeToRefs } from 'pinia';

const todoStore = useTodoStore();
const { todos, incompleteTodosCount } = storeToRefs(todoStore);

const newTodo = ref('');

const addTodo = () => {
  if (newTodo.value.trim()) {
    todoStore.addTodo(newTodo.value.trim());
    newTodo.value = '';
  }
};

const removeTodo = (todo) => {
  todoStore.removeTodo(todo);
};

const pesan = ref('');
watch(newTodo, (newTodoValue) => {
  if (newTodoValue.includes('?')) {
    pesan.value = 'Tidak boleh mengandung ?';
  } else {
    pesan.value = '';
  }
});
</script>

<template>

<nav id="navbar">
        <h1>Randy Ivando</h1>
        <ul>
        <li><a href="#">Home</a></li> 
        <li><a href="#">Profile</a></li>
        <li><a href="#">About</a></li>
        <li><a href="#">Contact</a></li>
        </ul>
    </nav>
  <div class="todo-container">
    <div class="todo-app">
      <h1>TODO LIST WITH PINIA</h1>
      <form @submit.prevent="addTodo" class="todo-form">
        <input v-model="newTodo" required placeholder="New todo" class="todo-input">
        <button type="submit" class="todo-button">submit</button>
      </form>

      <ul class="todo-list">
        <li v-for="todo in todos" :key="todo.id" class="todo-item">
          <input type="checkbox" v-model="todo.done" class="todo-checkbox">
          <span :class="{ done: todo.done }" class="todo-text">{{ todo.text }}</span>
          <button @click="removeTodo(todo)" class="delete-button">X</button>
        </li>
      </ul>

      <p class="pesan">{{ pesan }}</p>
      <p class="incomplete-tasks">todo yang selesai {{ incompleteTodosCount }}</p>
    </div>
  </div>
</template>


<style scoped>
h1{
  text-align: center;
}


.todo-container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background-color: #cecece;
  border-radius: 12px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  text-align: center;
  margin-top: 100px;
}

.todo-app {
  width: 100%;
  text-align: left;
}

.todo-form {
  margin-bottom: 20px;
}

.todo-input {
  width: calc(100% - 20px);
  padding: 12px;
  margin-bottom: 10px;
  border: none;
  background-color: #f9f9f9;
  border-radius: 6px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  transition: background-color 0.3s, box-shadow 0.3s;
  color: black;
}

.todo-input:focus {
  background-color: #fff;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.todo-input::placeholder {
  color: black;
}

.todo-button {
  background-color: #00c6e0;
  color: rgb(41, 41, 41);
  border: none;
  padding: 12px 20px;
  cursor: pointer;
  border-radius: 6px;
  transition: background-color 0.3s;
  display: block;
  margin: 0 auto;
}

.todo-button:hover {
  background-color: #036e15;
}

.todo-list {
  list-style-type: none;
  padding: 0;
}

.todo-item {
  background-color: #fff;
  padding: 10px;
  margin-bottom: 10px;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  display: flex;
  align-items: center;
}

.todo-checkbox {
  margin-right: 10px;
}

.todo-text {
  flex: 1;
  font-size: 18px;
  color: #333;
}

.todo-text.done {
  text-decoration: line-through;
  color: #999;
}

.delete-button {
  background-color: #ff1100;
  color: white;
  border: none;
  padding: 8px 12px;
  cursor: pointer;
  border-radius: 6px;
  transition: background-color 0.3s;
  margin-left: 10px;
}

.delete-button:hover {
  background-color: #B71C1C;
}

.pesan {
  font-style: italic;
  color: #F44336;
  text-align: center;
}

.incomplete-tasks {
  margin-top: 10px;
  font-weight: bold;
  text-align: center;
}
</style>

<style>

body{
  background-color: #f9f9f9;
}

#navbar h1 {
    --c:linear-gradient(#ff0000 0 0); /* update the color here */
  
    padding-bottom: .15em;
    background: var(--c), var(--c);
    background-size: .3em .1em;
    background-position:50% 100%;
    background-repeat: no-repeat;
    transition: .3s linear, background-size .3s .2s linear;
    color: #222222;
    margin-left: 20px;
    font-family:'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
    cursor: pointer;
}

#navbar h1:hover{
    background-size: 40% .1em;
    background-position: 10% 100%, 90% 100%;
}

#navbar{
    background-color: rgb(209, 209, 209);
    display: flex; 
    justify-content: space-between;
    
}
#navbar ul {
    display: flex;
    list-style: none;
    align-items: center;
    gap: 1rem 2rem;
    margin-right: 30px;
}

#navbar ul li a {
    text-decoration: none;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    font-weight: 600;
    padding:  3px 0px;
    border-bottom: 2px solid rgba(255, 0, 0, 0);
    transition: all;
    transition-duration: 300ms;
    position: relative;
    color: #222222;
    cursor: pointer;
}


#navbar ul li a::before{
    content: '';
    position: absolute;
    top: 100%;
    left: 0;
    width: 0;
    height: 2.5px;
    background: red;
    transition: 0.3s;
}

#navbar ul li a:hover::before{
    width: 100%;
}

</style>