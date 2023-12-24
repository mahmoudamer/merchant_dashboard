<template>
  <div>
    <div
      class="lg:hidden flex justify-between items-center py-3 px-6 border-b-2 border-gray-300"
    >
      <button @click="toggleSidebar" class="text-gray-600 text-lg">☰</button>
      <div class="text-2xl font-semibold text-blue-950">Dashboard</div>
    </div>
    <transition name="slide">
      <div
        v-show="isSidebarOpen || isLargeScreen"
        class="fixed top-0 left-0 w-full h-screen bg-blue-950 text-white z-50 lg:relative lg:w-auto"
      >
        <div class="p-4">
          <div class="text-2xl font-semibold mb-4">Dashboard</div>
          <NuxtLink
            @click="closeSidebar"
            to="/"
            class="block mb-2 hover:text-gray"
            >Profile</NuxtLink
          >
          <NuxtLink @click="closeSidebar" to="/transactions" class="block mb-2"
            >Transactions</NuxtLink
          >
        </div>
      </div>
    </transition>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from "vue";

const isSidebarOpen = ref(false);
const isLargeScreen = ref(false);

const toggleSidebar = () => {
  isSidebarOpen.value = !isSidebarOpen.value;
};

const closeSidebar = () => {
  if (!isLargeScreen.value) {
    isSidebarOpen.value = false;
  }
};

const checkScreenSize = () => {
  isLargeScreen.value = window.innerWidth >= 1024;
};

onMounted(() => {
  window.addEventListener("resize", checkScreenSize);
  checkScreenSize();
});

onUnmounted(() => {
  window.removeEventListener("resize", checkScreenSize);
});
</script>

<style scoped>
.slide-enter-active,
.slide-leave-active {
  transition: all 0.3s ease;
}
.slide-enter-from,
.slide-leave-to {
  transform: translateX(-100%);
}
.slide-enter-to,
.slide-leave-from {
  transform: translateX(0);
}

.router-link-exact-active {
  color: orange;
}
</style>
