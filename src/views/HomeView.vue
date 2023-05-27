<template >
  <div class="md:container md:mx-auto mx-4">
    <Navbar />
    <div
      class="md:flex md:justify-between md:space-x-5 md:space-y-0 space-y-7 py-10"
    >
      <AddTask @newTask="handleNewTask" />
      <InfoBox
        :totalTasks="this.tasks.length"
        :completedTasks="getCompleted"
        @showCompleted="showCompleted"
      />
    </div>
    <div class="shadow-2xl">
      <ul>
        <li v-for="task in filteredTasks" :key="task.id">
          <SingleTask
            :task="task"
            @removeTask="handleRemove"
            @completeTask="handleComplete"
            @editTask="handleEditedTask"
          />
        </li>
      </ul>
    </div>
  </div>
</template>
<script>
import Navbar from "@/components/Navbar.vue";
import AddTask from "@/components/AddTask.vue";
import InfoBox from "@/components/InfoBox.vue";
import SingleTask from "@/components/SingleTask.vue";

export default {
  name: "HomeView",
  data() {
    return {
      tasks: JSON.parse(localStorage.getItem("tasks")) || [],
      hideCompleted: false,
    };
  },
  components: {
    Navbar,
    AddTask,
    InfoBox,
    SingleTask,
  },
  methods: {
    handleNewTask(task) {
      this.tasks = [task, ...this.tasks];
    },
    handleComplete(id) {
      this.tasks = this.tasks.map((task) =>
        task.id === id ? { ...task, completed: !task.completed } : task
      );
    },
    handleEditedTask(updatedTask) {
      this.tasks = this.tasks.map((task) =>
        task.id === updatedTask.id ? { ...task, ...updatedTask } : task
      );
    },
    handleRemove(id) {
      this.tasks = this.tasks.filter((task) => task.id !== id);
    },
    showCompleted(show) {
      this.hideCompleted = show;
    },
  },
  computed: {
    filteredTasks() {
      return this.hideCompleted
        ? this.tasks.filter((task) => !task.completed)
        : this.tasks;
    },
    getCompleted() {
      return this.tasks.filter((task) => task.completed).length;
    },
  },
  watch: {
    tasks: {
      handler(tasks) {
        localStorage.setItem("tasks", JSON.stringify(tasks));
      },
    },
  },
};
</script>
