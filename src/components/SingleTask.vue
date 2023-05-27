<template>
  <div
    v-if="!isBeingEdited"
    class="flex items-center gap-11 p-10 shadow"
    :class="task.completed ? 'bg-gradient-to-r from-blue-200 ' : 'bg-none'"
  >
    <input
      type="checkbox"
      :checked="task.completed"
      title="Mark as complete"
      class="checkbox checkbox-info"
      @input="$emit('completeTask', task.id)"
    />
    <div class="flex-grow">
      <h3
        class="text-2xl cursor-pointer font-bold capitalize break-all"
        v-text="task.title"
        @click="showTaskDetails = !showTaskDetails"
      ></h3>
      <div v-show="showTaskDetails">
        <div class="divider"></div>
        <p class="break-all" v-text="task.details"></p>
      </div>
    </div>

    <div class="flex flex-col gap-10">
      <Icon
        icon="line-md:edit"
        width="32"
        height="32"
        class="cursor-pointer"
        @click="isBeingEdited = true"
      />
      <Icon
        icon="line-md:remove"
        width="32"
        height="32"
        class="cursor-pointer"
        @click="$emit('removeTask', task.id)"
      />
    </div>
  </div>
  <div v-else>
    <form
      @submit.prevent="handleEdit"
      class="flex items-center gap-11 p-10 shadow"
      :class="task.completed ? 'bg-gradient-to-r from-blue-200 ' : 'bg-none'"
    >
      <input
        type="checkbox"
        :checked="task.completed"
        title="Mark as complete"
        class="checkbox checkbox-info"
        @click="$emit('completeTask', task.id)"
      />
      <div class="flex-grow">
        <input
          class="text-2xl w-full border-2 border-black"
          v-model.trim="task.title"
          maxlength="50"
          required
        />

        <div class="divider"></div>
        <textarea
          class="break-all textarea textarea-bordered h-24 w-full resize-none border-2 border-black"
          v-model.trim="task.details"
          maxlength="250"
          required
        ></textarea>
        <div class="divider"></div>
        <div
          class="flex flex-col md:flex md:flex-row gap-3 justify-center items-center"
        >
          <button
            class="btn btn-info w-1/3"
            v-show="
              cachedTask.title !== task.title ||
              cachedTask.details !== task.details
            "
          >
            Save
          </button>
          <button class="btn btn-info w-1/3" @click.prevent="cancelEdit">
            Cancel
          </button>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
import { Icon } from "@iconify/vue";
export default {
  name: "SingleTask",
  data() {
    return {
      showTaskDetails: true,
      isBeingEdited: false,
      isUnchanged: null,
      cachedTask: {
        title: this.task.title,
        details: this.task.details,
      },
    };
  },
  components: {
    Icon,
  },
  props: {
    task: Object,
  },
  methods: {
    handleEdit() {
      if (this.isUnchanged) {
        this.isBeingEdited = false;
        return;
      }
      let editedTask = {
        id: this.task.id,
        title: this.task.title,
        details: this.task.details,
        completed: this.task.completed,
      };
      this.$emit("editTask", editedTask);
      this.isBeingEdited = false;
      this.cachedTask.title = this.task.title;
      this.cachedTask.details = this.task.details;
    },
    cancelEdit() {
      const { title, details } = this.cachedTask;
      Object.assign(this.task, { title, details });
      this.isBeingEdited = false;
    },
  },
  computed: {
    beforeEdit() {
      let checkBeforeEdit =
        this.cachedTask.title === this.task.title &&
        this.cachedTask.details === this.task.details;
      this.isUnchanged = checkBeforeEdit;
      return this.isUnchanged;
    },
  },
};
</script>
