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
      />
    </TransitionRoot>
  </div>
</template>
<script setup lang="ts">
import { ref } from "vue";
import { PlusIcon } from "@heroicons/vue/24/outline";
import { TransitionRoot } from "@headlessui/vue";
const showNotes = ref(false);
const { pending, data: savedNotes } = await useAuthLazyFetch(
  "https://v1-orm-lib.mars.hipso.cc/notes/entity/CONTACTS/1?project_id=12&offset=0&limit=100&sort_column=id&sort_direction=desc"
);
console.log("savedNotes", savedNotes);
</script>
