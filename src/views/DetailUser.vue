<script setup>
  import { reactive, onMounted } from 'vue';

  const state = reactive(() => {
    user: {}
  });

  onMounted(() => {
    console.log("mount detail");
    state.user = $route.params.user
  })


</script>

<template>
  <div
  >
    <header class="bg-white shadow" v-if="$route.meta.title">
      <div class="mx-auto max-w-7xl px-4 py-6 sm:px-6 lg:px-8">
        {{ $route.meta.title }}
      </div>
    </header>
    <div class="flex flex-col md:flex-row space-x-0 space-y-10 lg:space-x-10 lg:space-y-0">
      <div class="w-full lg:w-54">
        <img class="rounded-full w-16 h-auto border border-gray-100 shadow-sm" :src="''" alt="user image" />
      </div>
    </div>
  </div>
</template>
