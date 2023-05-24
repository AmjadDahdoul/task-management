<template>
  <div
    class="flex flex-col items-center justify-center rounded-2xl shadow-2xl p-5 font-mono font-bold"
  >
    <h3>Total Tasks: {{ totalTasks }}</h3>
    <div class="divider"></div>
    <h3>Completed Tasks {{ completedTasks }}</h3>
    <button
      class="btn btn-info my-4 btn-wide font-bold"
      @click="handleCompleted"
    >
      {{ hideCompleted ? "Show all" : "Hide completed" }}
    </button>
  </div>
</template>

<script>
export default {
  name: "InfoBox",
  data() {
    return {
      hideCompleted: false,
    };
  },
  props: {
    totalTasks: Number,
    completedTasks: Number,
  },
  methods: {
    handleCompleted() {
      this.hideCompleted = !this.hideCompleted;
      this.$emit("showCompleted", this.hideCompleted);
    },
  },
};
</script>
