<template>
  <div class="text-left">
    <button type="button" class="inline-flex items-center px-4 py-2 border border-transparent text-sm font-medium rounded-md shadow-sm text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
      Add Note
    </button>
  </div>
  <div v-for="note in notes" :key="note.id">
    <note-element :title="note.title" :description="note.description" :date="note.date"/>
  </div>
</template>

<script>
import NoteElement from "../components/NoteElement.vue";
import axios from "axios";
import { computed, ref } from 'vue';

export default {
  components: {
    NoteElement
  },
  setup () {
    const notes = ref([]);
    const loadingState = ref(null);

    // const notes = [
    //   {
    //     'title': 'Test 1',
    //     'description': 'Description goes here',
    //     'date': '10:53am 2020/03/03'
    //   },
    //   {
    //     'title': 'Test 2',
    //     'description': 'Description goes here',
    //     'date': '10:53am 2020/03/03'
    //   }
    // ]

    const fetchAllNotes = () => {
      loadingState.value = 'loading'
      return axios.get('https://notes.test/api/notes')
        .then(response => {
          loadingState.value = 'success'
          notes.value = response.data
        })
    }
    return { notes, fetchAllNotes }
  },
  created () {
    this.fetchAllNotes()
  }
}
</script>

<style>

</style>