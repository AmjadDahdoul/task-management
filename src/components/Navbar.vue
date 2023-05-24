<template>
  <nav class="my-4 text-center">
    <h3 class="text-3xl font-bold">Task Management</h3>
  </nav>
</template>

<script>
import { onMounted } from "vue";
import { themeChange } from "theme-change";
export default {
  name: "Navbar",

  setup() {
    onMounted(() => {
      themeChange(false);
    });
  },
};
</script>
