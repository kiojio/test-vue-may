<script setup>
import { ref, onMounted, onUnmounted, reactive } from 'vue'
import User from '@/components/User.vue'
import axios from 'axios'

const counter = ref(0)
const temp = ref([])
const state = reactive({
  dataList: [],
  search: ''
})
const url = "https://randomuser.me/api/"

let interval = setInterval(() => {
  counter.value++
  fetchData();
}, 10000);

const fetchData = async () => {
  await axios.get(url+'?results=20').then(resp => {
    const response = resp.data;
    const data = response.results
    const mergeData = [...state.dataList, ...data];
    state.dataList = [...new Map(mergeData.map(item =>
    [item['id'], item])).values()];
    console.log("respo", {res: response.results, resp: state.dataList});
  });
}

 const handleSearchInput = event => {
   console.log("ada", state);
   if(state.search.length == '' && temp.value.length > 0) {
      state.dataList = temp.value;
   }

   if(state.search.length >= 3 ) {
      const newData = state.dataList.filter(item =>
        item.name.first.toLowerCase().includes(event.target.value) ||
        item.name.last.toLowerCase().includes(event.target.value)
      )
      temp.value = state.dataList;
      state.dataList = newData;
   }
  };

onMounted(() => {
  console.log("mount");
  fetchData();
});

onUnmounted(() => {
  clearInterval(interval);
})


</script>

<template>
  <div class="bg-gray-300">
    <header class="bg-white shadow" v-if="$route.meta.title">
      <div class="mx-auto max-w-7xl px-4 py-6 sm:px-6 lg:px-8">
        <h1
          @click="counter = 0"
          class="text-3xl font-bold leading-tight text-gray-900"
        >
          {{ $route.meta.title }} / {{ counter }}
        </h1>
        <div class="flex justify-center m-2">   
          <label
            for="search"
            class="relative text-gray-400 focus-within:text-gray-600 w-full block"
          >
            <input v-model="state.search" @input="handleSearchInput" type='text' name="search" id="search" placeholder="Search"
              class="px-8 w-full border rounded py-2 text-gray-700 focus:outline-none"/>
            <button
              type="submit"
              class="pointer-events-none w-8 h-8 absolute top-1/2 transform -translate-y-1/2 left-3"
            >
              <svg
                width="24"
                height="24"
                viewBox="0 0 24 24"
                fill="none"
                xmlns="http://www.w3.org/2000/svg"
              >
                  <path
                      d="M19.023 16.977C18.51 16.489 18.019 15.98 17.656 15.593C17.284 15.215 17.06 14.94 17.06 14.94L14.26 13.603C15.34 12.37 16 10.763 16 9C16 5.141 12.86 2 9 2C5.14 2 2 5.141 2 9C2 12.859 5.14 16 9 16C10.763 16 12.37 15.34 13.603 14.261L14.94 17.061C14.94 17.061 15.215 17.285 15.593 17.657C15.98 18.02 16.489 18.511 16.977 19.024C17.471 19.53 17.965 20.036 18.335 20.416C18.697 20.804 18.939 21.062 18.939 21.062L21.06 18.941C21.06 18.941 20.802 18.699 20.414 18.337C20.035 17.965 19.529 17.471 19.023 16.977ZM9 14C6.243 14 4 11.757 4 9C4 6.243 6.243 4 9 4C11.757 4 14 6.243 14 9C14 11.757 11.757 14 9 14Z"
                      fill="currentColor"
                  />
              </svg>
            </button>
          </label>
        </div>
      </div>
    </header>
    <main>
      <div class="grid grid-cols-8 gap-2 m-2">
        <div class="col-span-12 lg:col-span-12">
          <div class="grid grid-cols-2 md:grid-cols-5 gap-2">
            <User
              v-for="data in state.dataList"
              :user="data"
            />
          </div>
        </div>
      </div>
    </main>
  </div>
</template>
