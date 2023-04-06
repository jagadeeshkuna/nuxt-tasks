<template>
  <div>
    <h3>Click on plus button to add notes and veiw notes</h3>
    <button
      type="button"
      class="rounded-full bg-blue-600 p-3 text-white shadow-sm hover:bg-blue-600 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600 fixed bottom-7 right-7"
      @click="showNotes = true"
    >
      <PlusIcon class="h-9 w-9" aria-hidden="true" />
    </button>
    <TransitionRoot as="template" :show="showNotes">
      <CollectionNotesList
        @hideSidebar="showNotes = false"
        :savedNotes="savedNotes"
        :textInput="textInput"
        @saveNotes="saveNotes"
        @deleteNotes="deleteNotes"
      />
    </TransitionRoot>
  </div>
</template>
<script setup lang="ts">
import { ref } from "vue";
import { PlusIcon } from "@heroicons/vue/24/outline";
import { TransitionRoot } from "@headlessui/vue";
const showNotes = ref(false);
const textInput = ref("");
const { pending, data: savedNotes } = await useAuthLazyFetch(
  "https://v1-orm-lib.mars.hipso.cc/notes/entity/CONTACTS/1?project_id=12&offset=0&limit=100&sort_column=id&sort_direction=asc"
);
const saveNotes = async (data: Object) => {
  let options = {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      Accept: "application/json",
      Authorization:
        "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1IjoiYzljMGM3MjkzMWI0NGZjOWE1NTc5ZWMyOTg4NzVlYzMiLCJkIjoiMTY4MDA2MiIsInIiOiJzYSIsInAiOiJmcmVlIiwiYSI6ImZpbmRlci5pbyIsImwiOiJ1czEiLCJleHAiOjE2ODMyNjczNzl9.3_zIDyeZ0ACxOF0VpywmxGpzdhUadzmvMRggb106s5E",
    },
    body: JSON.stringify(data),
  };
  const addNotesData = await useAuthLazyFetchPost(
    "https://v1-orm-lib.mars.hipso.cc/notes/CONTACTS/1",
    options
  );
  savedNotes.value.push(addNotesData.data.value);
};
const deleteNotes = async (data) => {
  const response = await useAuthLazyFetchDelete(
    `  https://v1-orm-lib.mars.hipso.cc/notes/${data.selectedNote.uid}`,
    {}
  );
  if (response.data.value) savedNotes.value.splice(data.index, 1);
};
</script>
