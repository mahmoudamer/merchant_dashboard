<template>
  <div class="lg:flex flex-none">
    <div class="lg:w-1/5 w-full">
      <Sidebar />
    </div>

    <div class="w-4/5 p-12">
      <slot />
    </div>
  </div>
</template>

<script>
import Sidebar from "@/components/layout/Sidebar/index.vue";

export default {
  components: {
    Sidebar,
  },
};
</script>
