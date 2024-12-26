<!-- Ternary operation 
v-bind:style= "[condition ? {style_A} : {style_B}]"

<div 
    :class="{'radar__container':true,'inactive':inactive}"
    :style= "inactive ? getStyleRadarContainerInactive : getStyleRadarContainer"
  >
-->

<template>
  <main>
    <!-- heading -->
    <header>
      <img src="https://pinia.vuejs.org/logo.svg" alt="pinia logo" />
      <h1>Pinia Tasks App</h1>
    </header>

    <!-- new task form -->
    <div class="new-task-form">
      <TaskForm />
    </div>

    <!-- filter -->
    <nav class="filter">
      <button @click="filter = 'all'" :class="{ backgroundButton: filter === 'all' }">All tasks</button>
      <button @click="filter = 'favs'" :class="filter === 'favs' ? 'backgroundButton' : 'white'">Fav tasks</button>
    </nav>

    <!-- loading -->
    <div class="loading" v-if="loading">Loading tasks...</div>

    <!-- task list -->
    <div class="task-list" v-if="filter === 'all'">
      <p>You have {{ totalCount }} tasks left to do.</p>
      <div v-for="task in tasks" :key="task.id">
        <TaskDetails :task="task" />
      </div>
    </div>
    <div class="task-list" v-if="filter === 'favs'">
      <p>You have {{ favCount }} tasks in your favs list.</p>
      <div v-for="task in favs" :key="task.id">
        <TaskDetails :task="task" />
      </div>
    </div>

    <div class="wrapper">
      <button @click="taskStore.$reset" class="button">reset the state</button>
    </div>
  </main>
</template>

<script>
import TaskDetails from './components/TaskDetails.vue'
import TaskForm from './components/TaskForm.vue'

import { storeToRefs } from 'pinia'
import { ref } from 'vue'
import { useTaskStore } from './stores/TaskStore'

export default {
  components: { TaskDetails, TaskForm },
  setup() {
    const taskStore = useTaskStore()

    const { tasks, loading, favs, totalCount, favCount } = storeToRefs(taskStore)

    // fetch tasks
    taskStore.getTasks()

    const filter = ref('all')

    return { taskStore, filter, tasks, loading, favs, totalCount, favCount }
  },
}
</script>

<style>
.button {
  margin: 0.5rem;
  align-content: center;
  align-items: center;
  background-color: bisque;
}
.wrapper {
  text-align: center;
}
.backgroundButton {
  background: green;
}
.white {
  background: #fff;
}
</style>
