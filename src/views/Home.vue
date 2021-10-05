<template>
  <TransitionRoot as="template" :show="open">
    <Dialog as="div" class="fixed z-10 inset-0 overflow-y-auto" @close="open = false">
      <div class="flex items-end justify-center min-h-screen pt-4 px-4 pb-20 text-center sm:block sm:p-0">
        <TransitionChild as="template" enter="ease-out duration-300" enter-from="opacity-0" enter-to="opacity-100" leave="ease-in duration-200" leave-from="opacity-100" leave-to="opacity-0">
          <DialogOverlay class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity" />
        </TransitionChild>

        <!-- This element is to trick the browser into centering the modal contents. -->
        <span class="hidden sm:inline-block sm:align-middle sm:h-screen" aria-hidden="true">&#8203;</span>
        <TransitionChild as="template" enter="ease-out duration-300" enter-from="opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95" enter-to="opacity-100 translate-y-0 sm:scale-100" leave="ease-in duration-200" leave-from="opacity-100 translate-y-0 sm:scale-100" leave-to="opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95">
          <div class="inline-block align-bottom bg-white rounded-lg px-4 pt-5 pb-4 text-left overflow-hidden shadow-xl transform transition-all sm:my-8 sm:align-middle sm:max-w-lg sm:w-full sm:p-6">
            <div>
              <form class="space-y-8 divide-y divide-gray-200">
                <div class="space-y-8 divide-y divide-gray-200">
                  <div>
                    <div>
                      <h3 class="text-lg leading-6 font-medium text-gray-900">
                        ADD NOTE
                      </h3>
                    </div>


                    <div class="sm:grid sm:grid-cols-3 sm:gap-4 sm:items-start sm:border-t sm:border-gray-200 sm:pt-5">
                      <label for="note-title" class="block text-sm font-medium text-gray-700 sm:mt-px sm:pb-2">
                        Title
                      </label>
                      <div class="mt-1 sm:mt-0 sm:col-span-2">
                        <input v-model="noteTitle" type="text" name="note-title" id="note-title" autocomplete="Note Title" class="max-w-lg block w-full shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:max-w-xs sm:text-sm border-gray-300 rounded-md" />
                      </div>
                    </div>

                    <div class="sm:grid sm:grid-cols-3 sm:gap-4 sm:items-start sm:border-t sm:border-gray-200 sm:pt-5">
                      <label for="note-description" class="block text-sm font-medium text-gray-700 sm:mt-px sm:pt-2">
                        Description
                      </label>
                      <div class="mt-1 sm:mt-0 sm:col-span-2">
                        <textarea v-model="noteDescription" id="note-description" name="note-description" rows="3" class="max-w-lg shadow-sm block w-full focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm border border-gray-300 rounded-md" />
                      </div>
                    </div>

                  </div>
                </div>
              </form>
            </div>

            <div class="mt-5 sm:mt-6 sm:grid sm:grid-cols-2 sm:gap-3 sm:grid-flow-row-dense">
              <button type="button" class="w-full inline-flex justify-center rounded-md border border-transparent shadow-sm px-4 py-2 bg-indigo-600 text-base font-medium text-white hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500 sm:col-start-2 sm:text-sm" @click="addNote">
                Add Note
              </button>
              <button type="button" class="mt-3 w-full inline-flex justify-center rounded-md border border-gray-300 shadow-sm px-4 py-2 bg-white text-base font-medium text-gray-700 hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500 sm:mt-0 sm:col-start-1 sm:text-sm" @click="open = false" ref="cancelButtonRef">
                Cancel
              </button>
            </div>
          </div>
        </TransitionChild>
      </div>
    </Dialog>
  </TransitionRoot>
  <div class="text-left">
    <button @click="open = !open" type="button" class="inline-flex items-center px-4 py-2 border border-transparent text-sm font-medium rounded-md shadow-sm text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
      Add Note
    </button>
  </div>
  <div v-for="note in notes" :key="note.id">
    <note-element :title="note.title" :description="note.description" :date="note.date"/>
  </div>
</template>

<script>
import { computed, ref } from 'vue';
import NoteElement from "../components/NoteElement.vue";
import axios from "axios";
import { Dialog, DialogOverlay, DialogTitle, TransitionChild, TransitionRoot } from '@headlessui/vue'
import { CheckIcon } from '@heroicons/vue/outline'

export default {
  components: {
    NoteElement,
    Dialog,
    DialogOverlay,
    DialogTitle,
    TransitionChild,
    TransitionRoot,
    CheckIcon,
  },
  setup () {
    // modal state
    let open = ref(false)
    // items we're displaying
    const notes = ref([]);
    //const loadingState = ref(null);

    let noteTitle = ref(null);
    let noteDescription = ref(null);

    const addNote = () => {
      //loadingState.value = 'loading'
      console.log("Add note");
      // Set note post data
      return axios.post('https://notes.test/api/notes', {
        'title': noteTitle.value,
        'description': noteDescription.value,
      })
          .then(response => {
            //loadingState.value = 'success'
            // do something with the output?
            console.log("Post Response:", response.data);
            // refresh data
            fetchAllNotes()
            // reset the note input fields
            noteTitle.value = null;
            noteDescription.value = null;
            // close modal
            open.value = false;
          })
    }

    // Client side mock data
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
      //loadingState.value = 'loading'
      return axios.get('https://notes.test/api/notes')
        .then(response => {
          //loadingState.value = 'success'
          notes.value = response.data
        })
    }
    return { notes, fetchAllNotes, addNote, noteTitle, noteDescription, open }
  },
  created () {
    this.fetchAllNotes()
  }
}
</script>

<style>

</style>