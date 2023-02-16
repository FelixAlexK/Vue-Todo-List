<script setup>
  import {ref, computed, reactive} from 'vue'
  import AddTodo from './components/AddTodo.vue';
  import RemoveTodo from './components/RemoveTodo.vue'


  
  let id = 0;

  const header = ref('Todo-List')
  const todos = ref([]);
  const newTodo = ref('');
  const newTodoHighPriority = ref(false)
  const todoDone = ref(false)
  const hideCompleted = ref(false)
  const editing = ref(true)
  const reversed = ref(false)

  function doEdit(){
    editing.value = !editing.value
  }
  

  function addTodo() {
    todos.value.push({ id: id++, text: newTodo.value, priority: newTodoHighPriority.value, done: todoDone.value});
    newTodo.value = ''
    newTodoHighPriority.value = ''
  }

  function removeTodo(todo){
    todos.value =  todos.value.filter((t) => t !==todo )
  }

  function toggleDone(todo){
    todo.done = !todo.done
  } 

  const filteredTodos = computed(() =>{
    return hideCompleted.value ? todos.value.filter((t) => !t.done) : todos.value
  })

  const reverseTodos = computed(() =>{
    return [...todos.value].reverse()
  })

</script>

<template>
  <div class="header">
    <h1>{{ header }}</h1>
    <button v-if="todos.length >= 2" class="btn btn-primary" @click="reversed = !reversed">{{ reversed ? 'oldest first' : 'newest first' }}</button>
    <button class="btn btn-primary" @click="doEdit">{{ editing ? 'Lock' : 'Edit' }}</button>
  </div>

  <div  class="list">
    <AddTodo v-if="editing" v-model:modelValue="newTodo" v-model:priorityValue="newTodoHighPriority" @add-todo="addTodo()" >
      <template #addButton>
        {{ 'Add Todo' }}
      </template>
      <template #checkbox>
        <input v-model="newTodoHighPriority" type="checkbox">
        {{ 'High Priority' }}
      </template>
    </AddTodo>
  	<ul>
      <li v-for="todo in reversed ? reverseTodos : filteredTodos" :key="todo.id"  @click="toggleDone(todo)">
        <p :class="{priority: todo.priority, strikeout: todo.done}">{{ todo.text }}</p>
        <RemoveTodo @remove-todo="removeTodo(todo)">{{ 'X' }}</RemoveTodo>
      </li>
    </ul>
  </div>

  <p v-if="todos.length === 0">{{ 'Nothing to see' }}</p>
  <button class="btn btn-primary" v-if="editing" @click="hideCompleted = !hideCompleted">{{ hideCompleted ? 'Show all' : 'Hide Completed' }}</button>
  
  
</template>

