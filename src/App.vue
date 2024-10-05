<template>
  <main>
    <header>
      <img src="https://pinia.vuejs.org/logo.svg" alt="pinia logo" />
      <h1>Pinia 備忘錄</h1>
    </header>
    <!-- new task form -->
    <div class="new-task-form">
      <TaskForm />
    </div>

    <!-- filter -->
    <nav class="filter">
      <button @click="filter = 'all'">全部</button>
      <button @click="filter = 'favs'">已完成</button>
    </nav>

    <!-- loading -->
    <div class="loading" v-if="loading">Loading tasks...</div>
    <!-- task list -->
    <div class="task-list" v-if="filter === 'all'">
      <p>你有 {{ totalCount }} 項任務</p>
      <div v-for="task in tasks" :key="task.id">
        <TaskDetails :task="task" />
      </div>
    </div>
    <div class="task-list" v-if="filter === 'favs'">
      <p>你已完成了 {{ favCount }} 項任務</p>
      <div v-for="task in favs" :key="task.id">
        <TaskDetails :task="task" />
      </div>
    </div>
    <div class="reset">
      <button @click="taskStore.$reset">重置清單</button>
    </div>
    
  </main>
</template>

<script>
import TaskDetails from "./components/TaskDetails.vue";
import TaskForm from "./components/TaskForm.vue";
import { useTaskStore } from "./stores/taskStore";
import { ref } from "vue";
import { storeToRefs } from "pinia";

export default {
  components: { TaskDetails, TaskForm },
  setup() {
    const taskStore = useTaskStore();
    const { tasks, loading, favs, totalCount, favCount } = storeToRefs(taskStore);

    taskStore.getTasks();
    const filter = ref("all");
    return { taskStore, filter, tasks, loading, favs, totalCount, favCount };
  },
};
</script>
