<template>
<div class="p-10">
  <h1 class="text-5xl text-gray-700">Dogs give you all you need to know</h1>
  <div v-if="loaded" class="mt-10 flex flex-col gap-5">
    <div v-for="image in images" :key="image" class="h-64 w-64 border rounded">
      <img :src="image" alt="dog" class="h-48 w-full object-cover object-top"/>
      <div class="p-2">
        <p class="text-gray-500 text-xs break-words">{{image}}</p>
      </div>
    </div>
  </div>
  <div v-else class="mt-10 flex flex-col gap-5 animate-pulse">
    <div v-for="i in 3" :key="i" class="h-64 w-64 border rounded">
      <div class="h-48 bg-gray-100 rounded"></div>
      <div class="p-2">
        <p class="text-gray-500 text-xs bg-gray-100 rounded h-5"></p>
      </div>
    </div>
  </div>
</div>
</template>

<script lang="ts">
import { defineComponent, onMounted, ref } from 'vue';

function api<T>(url: string): Promise<T> {
  return fetch(url)
    .then(response => {
      if (!response.ok) {
        throw new Error(response.statusText)
      }
      return response.json() as Promise<T>
    })
}

export default defineComponent({
  name: 'App',
  setup () {
    const loaded = ref(false)
    const images = ref<string[]>([])

    onMounted(async () => {
      images.value = (await api<{message:string[]}>('https://dog.ceo/api/breeds/image/random/3')).message    
      loaded.value = true
    })

    return {loaded, images}
  }
})
</script>

<style>

</style>
