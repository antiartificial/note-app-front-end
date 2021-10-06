<template>
  <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
    <div class="pt-4 pb-4 px-4 sm:px-3 lg:pt-4 lg:pb-4 lg:px-4 border-black rounded">
      <div class="relative max-w-lg mx-auto divide-y-2 divide-gray-200 lg:max-w-7xl">
        <div>
          <h2 class="text-left text-3xl tracking-tight font-extrabold text-gray-900 sm:text-4xl">
            {{ title }}
          </h2>
          <p class="text-left text-sm text-gray-500">
            Created: {{ date }}
          </p>
          <div class="mt-3 sm:mt-4 lg:grid lg:grid-cols-2 lg:gap-5 lg:items-center">
            <p class="text-left text-xl text-gray-500">
              {{ description }}
            </p>
          </div>
          <div class="text-left py-5">
            <h3 class="text-2xl tracking-tight font-extrabold text-gray-900 sm:text-2xl">Tags</h3>
            <div class="flex py-5">
              <div v-for="(tag, index) in tags" :key="index" class="item-center">
                <div class="item-center mr-1 p-3 bg-green-900 rounded text-white">{{ tag.name }}</div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
//import { format } from "date-fns"
export default {
  props: ['title', 'description', 'tags', 'date'],
  setup() {
      // function formatDate(strDate) {
      //   return format(new Date(strDate), "hh:mma yyyy/MM/dd")
      // }
  }
}
</script>

<style>

</style>