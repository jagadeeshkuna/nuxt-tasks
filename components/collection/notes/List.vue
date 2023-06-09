<template>
  <Dialog as="div" class="relative z-10" @close="emit('hideSidebar')">
    <div class="fixed inset-0" />
    <div class="fixed inset-0 overflow-hidden">
      <div class="absolute inset-0 overflow-hidden">
        <div
          class="pointer-events-none fixed inset-y-0 right-0 flex max-w-full pl-10 sm:pl-16"
        >
          <TransitionChild
            as="template"
            enter="transform transition ease-in-out duration-500 sm:duration-700"
            enter-from="translate-x-full"
            enter-to="translate-x-0"
            leave="transform transition ease-in-out duration-500 sm:duration-700"
            leave-from="translate-x-0"
            leave-to="translate-x-full"
          >
            <DialogPanel class="pointer-events-auto w-screen max-w-2xl">
              <form
                class="flex h-full flex-col overflow-y-scroll bg-white shadow-xl"
              >
                <div class="flex-1">
                  <!-- Header -->
                  <div class="bg-gray-50 px-4 py-6 sm:px-6">
                    <div class="flex items-start justify-between space-x-3">
                      <div class="space-y-1">
                        <DialogTitle
                          class="text-base font-semibold leading-6 text-gray-900"
                          >Notes</DialogTitle
                        >
                      </div>
                      <div class="flex h-7 items-center">
                        <button
                          type="button"
                          class="text-gray-400 hover:text-gray-500"
                          @click="emit('hideSidebar')"
                        >
                          <span class="sr-only">Close panel</span>
                          <XMarkIcon class="h-6 w-6" aria-hidden="true" />
                        </button>
                      </div>
                    </div>
                  </div>
                </div>
                <div v-for="(notes, index) in savedNotes" :key="index">
                  <div class="group/item flex">
                    <div class="relative flex min-w-0 items-center mr-6 py-2">
                      <span class="relative inline-block flex-shrink-0">
                        <img
                          src="https://images.unsplash.com/photo-1502685104226-ee32379fefbe?ixlib=rb-=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=facearea&facepad=3&w=1024&h=1024&q=80"
                          alt=""
                          class="h-8 w-8 rounded-full"
                        />
                      </span>
                      <div class="ml-4 truncate">
                        <p class="truncate text-sm font-medium text-gray-900">
                          Jagadeesh Kuna
                        </p>
                        <p class="truncate text-sm text-gray-500">
                          {{ notes.note }}
                        </p>
                      </div>
                    </div>
                    <div class="group-hover/item:visible invisible flex">
                      <PencilIcon
                        class="h-[17px] cursor-pointer mr-[6px] pt-1"
                        @click="editNote(notes, index)"
                      />
                      <TrashIcon
                        class="h-[17px] cursor-pointer mr-[6px] pt-1 tect-danger"
                        @click="deleteNote(notes, index)"
                      />
                    </div>
                  </div>
                </div>
                <div
                  class="flex flex-row items-center h-16 bg-white w-full px-4 bottom-0"
                >
                  <div class="flex-grow ml-4">
                    <div class="relative w-full">
                      <input
                        id="noteInput"
                        v-model="textInput"
                        type="text"
                        class="flex w-full border rounded-xl focus:outline-none focus:border-indigo-300 pl-4 h-10"
                      />
                    </div>
                  </div>
                  <div class="ml-4" @click.prevent="addNotes()">
                    <button
                      class="flex items-center justify-center bg-indigo-500 hover:bg-indigo-600 rounded-xl text-white px-4 py-1 flex-shrink-0"
                    >
                      <span class="ml-2">
                        <svg
                          class="w-4 h-4 transform rotate-45 -mt-px"
                          fill="none"
                          stroke="currentColor"
                          viewBox="0 0 24 24"
                          xmlns="http://www.w3.org/2000/svg"
                        >
                          <path
                            stroke-linecap="round"
                            stroke-linejoin="round"
                            stroke-width="2"
                            d="M12 19l9 2-9-18-9 18 9-2zm0 0v-8"
                          ></path>
                        </svg>
                      </span>
                    </button>
                  </div>
                </div>
                <TransitionRoot as="template" :show="showDeleteModal">
                  <CollectionNotesDeleteModal @hideModal="deleteNotesData" />
                </TransitionRoot>
              </form>
            </DialogPanel>
          </TransitionChild>
        </div>
      </div>
    </div>
  </Dialog>
</template>

<script setup>
import { ref, defineEmits, defineProps } from "vue";
import {
  Dialog,
  DialogPanel,
  DialogTitle,
  TransitionChild,
  TransitionRoot,
} from "@headlessui/vue";
import { XMarkIcon } from "@heroicons/vue/24/outline";
import { PencilIcon, TrashIcon } from "@heroicons/vue/24/outline";

const emit = defineEmits(["hideSidebar", "deleteNotes"]);
const props = defineProps({
  // Declare props
  savedNotes: Array,
});
const textInput = ref("");
const showDeleteModal = ref(false);
const selectedNote = ref({});
const selectedIndex = ref(0);
const addNotes = () => {
  let data = {
    project_id: "12",
    entity_id: "1",
    note: `${textInput.value}`,
    entity: "CONTACTS",
  };
  emit("saveNotes", data);
  textInput.value = "";
};
const deleteNote = (note, index) => {
  showDeleteModal.value = true;
  selectedNote.value = note;
  selectedIndex.value = index;
};
const deleteNotesData = (type) => {
  showDeleteModal.value = false;
  type
    ? emit("deleteNotes", {
        selectedNote: selectedNote.value,
        selectedIndex: selectedIndex.value,
      })
    : "";
};
</script>
