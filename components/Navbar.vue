<template>
  <header class="contsiner bg-gray-200 p-2 sticky top-0 left-0 right-0 z-10">
    <div class="flex justify-between items-center md:px-8 py-4 px-2">
      <div class="flex items-center">
        <div class="h-16 w-16 md:h-20 md:w-20 rounded-full bg-black"></div>
        <div class="ml-4">
          <h1 class="text-lg md:text-2xl font-semibold">Welcome,</h1>
          <h1 class="text-xl font-semibold">{{ username }}.</h1>
          <h1 class="text-lg font-semibold">{{ kelas }}</h1>
        </div>
      </div>

      <button
        @click="logout"
        class="hidden xl:block px-4 py-2 bg-red-500 text-white rounded"
      >
        Logout
      </button>

      <button @click="toggleMenu" class="xl:hidden relative z-50">
        <component :is="isSidebarOpen ? IconX : IconMenu2"></component>
      </button>

      <div
        v-if="isSidebarOpen"
        @click="toggleMenu"
        class="fixed inset-0 bg-black opacity-30 z-40"
      ></div>

      <div :class="sidebarClasses">
        <div class="h-screen flex items-center justify-center">
          <div class="mx-auto">
            <div class="h-24 w-24 rounded-full bg-black"></div>

            <ul class="my-3">
              <li>
                <h1 class="text-xl font-semibold">{{ username }}</h1>
              </li>
              <li>
                <h1 class="text-2xl font-semibold">{{ kelas }}</h1>
              </li>
            </ul>

            <button
              @click="logout"
              class="px-4 py-2 bg-red-500 text-white rounded"
            >
              Logout
            </button>
          </div>
        </div>
      </div>
    </div>
  </header>
</template>

<script setup>
import { IconMenu2 } from "@tabler/icons-vue";
import { IconX } from "@tabler/icons-vue";

const isSidebarOpen = ref(false);
const username = ref("Rochmad Rizky Putra");
const kelas = ref("XII");

const logout = () => {
  alert("Logout");
};

const toggleMenu = () => {
  isSidebarOpen.value = !isSidebarOpen.value;
};

const sidebarClasses = computed(() => {
  return (
    "fixed top-0 left-0 h-full w-64 md:w-72 lg:w-[350px] bg-gray-200 text-black px-4 transform transition-transform duration-300 ease-in-out z-50" +
    (isSidebarOpen.value ? " translate-x-0" : " -translate-x-full")
  );
});
</script>
