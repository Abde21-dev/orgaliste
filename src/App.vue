
<template>
  <div class="container">
  <header>
    <h1>Orgaliste</h1>
  </header>
   <form action="" @submit.prevent="addTodo">
    <fieldset role="group">
      <input v-model="newTodo" type="text" placeholder="entrez une tâche">
      <button :disabled="newTodo.length === 0" @click="">Ajouter</button>
    </fieldset>
   </form>
      <div v-if="todos.length === 0">Il n'y a pas de tâches à faires</div>
      
      <div v-else> 
        
        <p v-if="remainingTodos > 0">
        {{ remainingTodos }} tâche{{ remainingTodos > 1 ? 's' : '' }} à faire.
      </p>
      <p v-else="remainingTodos = 0">
        Vous n'avez pas de tâches à faire.
      </p>

      <ul> 
        <TransitionGroup name="list">
        <li v-for="todo in sortedTodos" 
        :key="todo.date" 
        :class="{completed: todo.completed}"
        >
          <label>
            <input type="checkbox" v-model="todo.completed">
            {{ todo.title }}
            <button @click="deleteTodos(todo)">x</button>
          </label>
        </li>
      </TransitionGroup>
      </ul>
      <label>
        <input type="checkbox" v-model="hideCompleted">
        Masquer les tâches complétes
      </label>
    
      </div>
    </div>
   

  
</template>

<script setup>
import { computed, ref } from 'vue'


const hideCompleted = ref(false)

const newTodo = ref('')
const todos = ref([])



const addTodo  = () => {
  todos.value.push({
  title: newTodo.value,
  completed: false,
  date: Date.now()
  })
  newTodo.value = ''
}

const sortedTodos = computed(() => {
const sortedTodos = todos.value.toSorted((a, b) => a.completed 
> b.completed ? 1 : -1)
if (hideCompleted.value === true) {
  return sortedTodos.filter(t => t.completed === false)
}
return sortedTodos
})

const remainingTodos = computed(() => {
  return todos.value.filter(t => t.completed === false).length
}

)
const deleteTodos = (todo) => {
  todos.value = todos.value.filter(t => t != todo)
}
</script>

<style>
.completed {
opacity: .5;
text-decoration: line-through;
}

.container {
  margin-top: 2rem;
}
.list-enter-active,
.list-leave-active {
  transition: all 0.7s ease;
}



.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateX(30px);
}
</style>

