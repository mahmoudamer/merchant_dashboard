<template>
  <h1 class="text-3xl font-semibold text-gray-700 mb-7">{{ title }}</h1>
  <div class="">
    <div class="mb-4">
      <label for="name" class="block text-gray-600 text-lg font-bold mb-2"
        >Name:</label
      >
      <p id="name" class="text-gray-600">{{ name }}</p>
    </div>
    <div class="mb-4">
      <label for="gender" class="block text-gray-600 text-lg font-bold mb-2"
        >Gender:</label
      >
      <p id="gender" class="text-gray-600">{{ gender }}</p>
    </div>
    <div class="mb-4">
      <label for="dob" class="block text-gray-600 text-lg font-bold mb-2"
        >Date of Birth:</label
      >
      <p id="dob" class="text-gray-600">{{ dob }}</p>
    </div>
  </div>
</template>

<script setup lang="ts">
const title: string = "Profile";
let name: string = "Mahmoud amer";
let gender: string = "Male";
let dob: string = "2nd Mon 1992";
</script>

<style></style>
