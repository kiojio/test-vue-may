<script setup>
  import { onMounted, reactive } from 'vue';
  import dayjs from 'dayjs';
  import { useRoute, useRouter } from 'vue-router';

  const route = useRoute()
  const router = useRouter()
  const state = reactive({
    user: {}
  })

  onMounted(() => {
    console.log("mount detail");
    if(route.params.hasOwnProperty('user')) {
      state.user = JSON.parse(route.params.user)
    } else {
      router.back()
    }
  })


</script>

<template>
  <div>
    <header class="bg-white shadow" v-if="$route.meta.title">
      <div class="mx-auto max-w-7xl px-4 py-6 sm:px-6 lg:px-8">
        {{ $route.meta.title }}
      </div>
    </header>
    <div class="container mx-auto mt-4 p-6 border border-grey-40 rounded-md">
      <div v-if="state.user.hasOwnProperty('picture')" class="flex flex-col md:flex-row space-x-0 space-y-10 lg:space-x-10 lg:space-y-0">
        <div
          class="relative text-white"
        >
          <img width="350" class="h-auto border border-gray-100 shadow-sm" :src="state.user.picture.large" alt="user image" />
        </div>
        <div class="flex-grow-1">
          <div class="flex flex-col">
              <div class="h4 md:underline font-bold text-grey-60 align-middle pb-3">
                  Biodata
              </div>
              <table class="biodata">
                <tr>
                  <th>Full Name</th>
                  <td class="font-medium">
                    {{ (state.user && state.user.name.first + ' ' + state.user.name.last) || "-" }}
                  </td>
                </tr>
                <tr>
                  <th>Birthday</th>
                  <td class="font-medium">
                    {{
                      (state.user && dayjs(state.user.dob.date).format('DD/MM/YYYY')) || '-'
                    }}
                  </td>
                </tr>
              </table>
              <div class="h4 md:underline font-bold text-grey-60 align-middle pb-3 pt-4">
                  Contact
              </div>
              <table class="biodata">
                <tr>
                  <th>Email</th>
                  <td class="font-medium">
                    {{ (state.user && state.user.email) || "-" }}
                  </td>
                </tr>
                <tr>
                  <th>Cell Phone</th>
                  <td class="font-medium">
                    {{ (state.user && state.user.cell) || "-" }}
                  </td>
                </tr>
                <tr>
                  <th>Birthday</th>
                  <td class="font-medium">
                    {{
                      (state.user && state.user.phone) || '-'
                    }}
                  </td>
                </tr>
              </table>
              <div class="h4 md:underline font-bold text-grey-60 align-middle pb-3 pt-4">
                Location
              </div>
              <table class="biodata">
                <tr>
                  <th>Country</th>
                  <td class="font-medium">
                    {{ (state.user && state.user.location.country) || "-" }}
                  </td>
                </tr>
                <tr>
                  <th>State</th>
                  <td class="font-medium">
                    {{ (state.user && state.user.location.state) || "-" }}
                  </td>
                </tr>
                <tr>
                  <th>City</th>
                  <td class="font-medium">
                    {{
                      (state.user && state.user.location.city) || '-'
                    }}
                  </td>
                </tr>
                <tr>
                  <th>Coordinate</th>
                  <td class="font-medium">
                    {{
                      (state.user && state.user.location.coordinates.latitude + ', ' +state.user.location.coordinates.longitude) || '-'
                    }}
                  </td>
                </tr>
                <tr>
                  <th>Street</th>
                  <td class="font-medium">
                    {{
                      (state.user && state.user.location.street.number + ', ' + state.user.location.street.name) || '-'
                    }}
                  </td>
                </tr>
              </table>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style lang="postcss" scoped>
  .biodata {
    @apply text-left w-full;
  }

  .biodata tr th {
    @apply font-semibold;
    @apply inline-block;
    width: 40%;
  }

  .biodata tr td {
    @apply inline-block;
    width: 60%;
  }

  .biodata tr {
    @apply py-3;
    @apply w-full;
    @apply flex;
  }
</style>
