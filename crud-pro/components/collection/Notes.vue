<template>
  <button
    type="button"
    @click="openSidebar = true"
    class="rounded bg-indigo-500 ml-3 mt-5 px-2 py-1 text-sm font-semibold text-white shadow-sm hover:bg-indigo-400 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-500"
  >
    Notes
  </button>
<CollectionNotesList :openSidebar="openSidebar" />
</template>
<script setup lang="ts">
import { ref } from "vue";
const openSidebar = ref(false);
</script>
