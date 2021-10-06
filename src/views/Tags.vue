<template>
  <div class="mx-4">
    <div class="hidden sm:block" aria-hidden="true">
      <div class="py-5">
      </div>
    </div>

    <div class="mt-10 sm:mt-0">
      <div class="md:grid md:grid-cols-3 md:gap-6 text-left">
        <div class="md:col-span-1">
          <div class="px-4 sm:px-0">
            <h3 class="text-lg font-medium leading-6 text-gray-900">Tags</h3>
            <p class="mt-1 text-sm text-gray-600">
              Manage tags here.
            </p>
          </div>
        </div>
        <div class="mt-5 md:mt-0 md:col-span-2 text-left">
          <!-- <form action="#" method="POST"> -->
          <div>
            <div class="shadow overflow-hidden sm:rounded-md">
              <div class="px-4 py-5 bg-white sm:p-6">
                <div class="grid grid-cols-6 gap-6">
                  <div class="col-span-6 sm:col-span-6">
                    <label for="street-address" class="block text-sm font-medium text-gray-700">Create New Tag</label>
                    <input v-on:keyup.enter="createNewTag" v-model="newTagName" type="text" name="new-tag-name" id="new-tag-name" autocomplete="New tag name" class="mt-1 focus:ring-green-500 focus:border-green-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md" />
                  </div>

                  <div class="col-span-6 sm:col-span-3">
                    <div class="px-4 py-5 bg-white space-y-6 sm:p-6 shadow-sm sm:text-sm border-gray-300 rounded-md">
                      <fieldset>
                        <legend class="text-base font-medium text-gray-900">Tags</legend>
                        <div class="mt-4 space-y-4">
                          <div class="flex items-start" v-for="tag in tagsList" :key="tag.id">
                            <div  @click="beginRemoveTag(tag.id)" class="flex items-center h-5">
                              <input :id="`tag-${tag.id}`" :name="`tag-${tag.id}`" type="checkbox" class="focus:ring-green-500 h-4 w-4 text-green-600 border-gray-300 rounded" />
                            </div>
                            <div class="ml-3 text-sm">
                              <label class="font-medium text-gray-700">{{ tag.name }}</label>
                            </div>
                          </div>
                        </div>
                      </fieldset>
                    </div>
                  </div>

                </div>
              </div>
              <div class="px-4 py-3 bg-gray-50 text-right sm:px-6">
                <button type="submit" class="inline-flex justify-center py-2 px-4 border border-transparent shadow-sm text-sm font-medium rounded-md text-white bg-green-600 hover:bg-green-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-green-500">
                  Save
                </button>
              </div>
            </div>
          <!-- </form> -->
          </div>
        </div>
      </div>
    </div>

    <div class="hidden sm:block" aria-hidden="true">
      <div class="py-5">
      </div>
    </div>
  </div>

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
            <div class="sm:flex sm:items-start">
              <div class="mx-auto flex-shrink-0 flex items-center justify-center h-12 w-12 rounded-full bg-red-100 sm:mx-0 sm:h-10 sm:w-10">
                <ExclamationIcon class="h-6 w-6 text-red-600" aria-hidden="true" />
              </div>
              <div class="mt-3 text-center sm:mt-0 sm:ml-4 sm:text-left">
                <DialogTitle as="h3" class="text-lg leading-6 font-medium text-gray-900">
                  Tag Deletion
                </DialogTitle>
                <div class="mt-2">
                  <p class="text-sm text-gray-500">
                    Are you sure you want to delete this tag?
                  </p>
                </div>
              </div>
            </div>
            <div class="mt-5 sm:mt-4 sm:flex sm:flex-row-reverse">
              <button @click="deleteConfirmation" type="button" class="w-full inline-flex justify-center rounded-md border border-transparent shadow-sm px-4 py-2 bg-red-600 text-base font-medium text-white hover:bg-red-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-red-500 sm:ml-3 sm:w-auto sm:text-sm">
                Yes, Delete
              </button>
              <button type="button" class="mt-3 w-full inline-flex justify-center rounded-md border border-gray-300 shadow-sm px-4 py-2 bg-white text-base font-medium text-gray-700 hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-green-500 sm:mt-0 sm:w-auto sm:text-sm" @click="open = false" ref="cancelButtonRef">
                Cancel
              </button>
            </div>
          </div>
        </TransitionChild>
      </div>
    </Dialog>
  </TransitionRoot>
</template>

<script>
import { ref } from 'vue'
import { Dialog, DialogOverlay, DialogTitle, TransitionChild, TransitionRoot } from '@headlessui/vue'
import { ExclamationIcon } from '@heroicons/vue/outline'
import axios from "axios";

export default {
  components: {
    Dialog,
    DialogOverlay,
    DialogTitle,
    TransitionChild,
    TransitionRoot,
    ExclamationIcon,
  },
  setup() {
    let open = ref(false)
    let newTagName = ref(null)
    let tagsList = ref([])
    let tagSelected = ref(null);

    // Mock data
    // tagsList = [
    //   {
    //     "id": 1,
    //     "name": "Tag 1"
    //   },
    //   {
    //     "id": 2,
    //     "name": "Tag 2"
    //   },
    //   {
    //     "id": 3,
    //     "name": "Tag 3"
    //   },
    // ]

    function createNewTag() {
      let tagName = newTagName.value
      // remove leading and trailing whitespace
      tagName = tagName.trim()
      // poor man's input validation
      if(tagName.length > 0) {
        console.log("valid tag!")
        addTag()
      }
      //console.log("new tag event")
    }

    // save tag to db
    const addTag = () => {
      //loadingState.value = 'loading'
      console.log("Add tag");
      // Set note post data
      return axios.post(import.meta.env.VITE_API_URL+'tags', {
        'name': newTagName.value,
      })
          .then(response => {
            //loadingState.value = 'success'
            // do something with the output?
            console.log("Post Response:", response.data);

            // refresh data
            fetchAllTags()

            // reset the note input fields
            newTagName.value = null;
          })
    }

    function beginRemoveTag(tagId) {
      tagSelected.value = tagId
      console.log("begin remove tag event:", tagId)
      open.value = true;
    }

    const deleteTag = () => {
      //loadingState.value = 'loading'
      console.log("Delete tag");
      // Set note post data
      return axios.delete(import.meta.env.VITE_API_URL+`tags/${tagSelected.value}`, {
        'name': newTagName.value,
      })
          .then(response => {
            //loadingState.value = 'success'
            // do something with the output?
            console.log("Delete Response:", response.data);

            // refresh data
            fetchAllTags()
          })
    }

    function confirmRemoveTag() {
      open.value = false;
      console.log("confirm delete event:", tagSelected.value)
      deleteTag()
      return true;
    }

    // get tags from db
    const fetchAllTags = () => {
      //loadingState.value = 'loading'
      return axios.get(import.meta.env.VITE_API_URL+'tags')
          .then(response => {
            //loadingState.value = 'success'
            tagsList.value = response.data
          })
    }

    return {
      newTagName, createNewTag, fetchAllTags, tagsList, beginRemoveTag, open, deleteConfirmation: confirmRemoveTag
    }
  },
  created() {
    this.fetchAllTags()
  }
}
</script>
<style></style>