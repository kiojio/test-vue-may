<script setup>
import { defineProps, toRefs, reactive } from 'vue';
import dayjs from 'dayjs';

const props = defineProps({
  user: Object
})
const { 
  user
} = toRefs(props);

const state = reactive({
  active: 'profile',
  info: 'My Name is ' + user.value.name.first + ' ' + user.value.name.last
})

const showInfo = (infoActive) => {
  console.log("check", user.value);
  const userData = user.value;
  switch(infoActive) {
    case 'profile':
      if(state.active == infoActive) {
        state.active = '';
        state.info = '';
      } else {
        state.active = infoActive;
        state.info = 'My Name is ' + userData.name.first + ' ' + userData.name.last;
      }
      break;
    case 'calendar':
      if(state.active == infoActive) {
        state.active = '';
        state.info = '';
      } else {
        state.active = infoActive;
        state.info = 'My Birthday is \n' + dayjs(userData.dob.date).format('DD/MM/YYYY');
      }
      break;
    case 'map':
      if(state.active == infoActive) {
        state.active = '';
        state.info = '';
      } else {
        state.active = infoActive;
        state.info = 'My Address is \n' + userData.location.street.number + ' ' +userData.location.street.name;
      }
      break;
    default:
  }
}

</script>

<template>
  <div
    class="flex flex-col bg-white shadow-sm w-full h-auto rounded-md border-2 items-center p-2"
  >
    <img class="rounded-full w-16 h-auto border border-gray-100 shadow-sm" :src="user.picture.thumbnail" alt="user image" />
    <p class="p-3">{{state.info}}</p>
    <div class="self-center grid grid-cols-4 md:grid-cols-4 gap-2">
      <button
        @click="showInfo('profile')"
      >
        <img :class="state.active == 'profile' ? 'border-t-4 border-amber-500' : ''" :src="'src/assets/profile.png'" width="40"/>
      </button>
      <button
        @click="showInfo('calendar')"
      >
        <img :class="state.active == 'calendar' ? 'border-t-4 border-amber-500' : ''" :src="'src/assets/calendar.png'" width="40"/>
      </button>
      <button
        @click="showInfo('map')"
      >
        <img :class="state.active == 'map' ? 'border-t-4 border-amber-500' : ''" :src="'src/assets/map.png'" width="40"/>
      </button>
      <router-link
        :to="{name: 'detailUser', params: {user: user}}"
        class="btn btn-blue"
      >Detail</router-link>
    </div>
  </div>
</template>
