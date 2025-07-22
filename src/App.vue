<script setup lang="ts">
import { onMounted, ref } from 'vue';

const notes = ref([
  { id: 1, createdAt: '01.01.1970 00:00', updatedAt: '01.01.01.1970 00:00', content: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit.' },
]);

const error = ref('');
const _error = (err: any) => {
  console.log(err);
  error.value = err.toString();
};

onMounted(() => {
  fetch('https://api.lanny.dev/notes')
      .then(res => res.json())
      .then(data => notes.value = data)
      .catch(err => _error(err))
});

const content = ref('');
const addNote = () => {
  fetch('https://api.lanny.dev/notes?content=' + encodeURIComponent(content.value), {
    method: 'POST',
  }).then(res => res.json())
    .then(data => {
      notes.value.unshift(data);
      content.value = '';
      error.value = '';
    }).catch(err => _error(err))
};
</script>

<template>
  <div class="flex w-full h-dvh flex-col content-center items-center p-5">
    <h1 class="text-5xl font-bold text-blue-500">Notes</h1>
    <h2 class="font-bold text-red-500">{{ error }}</h2>
    <div class="flex max-w-xl w-full p-5 gap-2">
      <input v-model="content" class="flex-1 bg-white text-gray-700 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent px-4 py-2" placeholder="What's on your mind?" />
      <button @click="addNote" type="button" class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-blue-300 font-medium rounded-md text-sm px-4 py-2">Add</button>
    </div>
    <div class="flex flex-col h-full w-full content-center items-center">
      <div class="flex flex-col w-full max-w-xl gap-3 border-b border-gray-200 pb-3 mb-3" v-for="note in notes">
        <p class="text-gray-500 font-bold text-sm">Created At: {{ note.createdAt }}</p>
        <p class="text-base text-gray-800">{{ note.content }}</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
</style>
