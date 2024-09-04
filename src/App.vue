<template>
  <form action="" @submit.prevent="AddTodo">
    <fieldset role="group">
      <input type="text" placeholder="Tâche à faire" v-model="newTodo">
      <button :disabled="newTodo.length == 0" >AJOUTER</button> <!-- :disabled permet d'empecher de soummtre le 
      formulaire si le champ ne contient pas de valeur -->
    </fieldset>
  </form>
    <div v-if="todos.length == 0">vous n'avez pas de tâche à faire </div>
    <div v-else>
      <ul>
        <li v-for="todo in sortedTodos()" :key="todo.data" :class="{completed: todo.completed}">
          <label></label> 
          <input type="checkbox" v-model="todo.completed">
          {{todo.title}}</li>
      </ul>
      <label>
        <input type="checkbox" v-model="hideCompleted">
        Masquer les tâche completées
      </label>
    </div>
</template>


<script setup>
  import { ref } from "vue"
  
  const newTodo = ref('')
  const hideCompleted = ref(false)
  const todos = ref([])
  const AddTodo = ()=>{
    todos.value.push({
      title: newTodo.value,
      completed: false,
      date: Date.now()
    })
    newTodo.value = ''
  }

  const sortedTodos = ()=>{
   const sortedTodos =  todos.value.toSorted((a,b) => a.completed > b.completed ? 1 : -1)
   if(hideCompleted.value == true){
     return sortedTodos.filter(t => t.completed == false)
   }
   return sortedTodos
  }
  
</script>

<style>
  .completed{
    opacity: .5;
    text-decoration: line-through;
  }
</style>