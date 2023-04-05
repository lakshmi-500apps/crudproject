<template>
  <TransitionRoot as="template" :show=" openSidebar">
    <Dialog as="div" class="relative z-10" @close="open = false">
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
                <div
                  class="flex h-full flex-col overflow-y-scroll bg-white py-6 shadow-xl"
                >
                  <div class="px-4 sm:px-6">
                    <div class="flex items-start justify-between">
                      <DialogTitle
                        class="text-base font-semibold leading-6 text-gray-900"
                        >Notes</DialogTitle
                      >
                      <div class="ml-3 flex h-7 items-center">
                        <button
                          type="button"
                          class="rounded-md bg-white text-gray-400 hover:text-gray-500 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2"
                          @click="open = false"
                        >
                          <span class="sr-only">Close panel</span>
                          <XMarkIcon class="h-6 w-6" aria-hidden="true" />
                        </button>
                      </div>
                    </div>
                  </div>
                  <div class="border ml-3 mr-3 relative mt-6 flex-1 px-4 sm:px-6">
                    
                    <section class="border mt-1" v-for="person in comment" :key="person.email">
                       
                        <div class="flex ">
        <div><img class="h-10 w-10 rounded-full mt-1 ml-2" :src="person.imageUrl" alt="" /></div><div class="mt-3 ml-2">{{person.name}}</div>
        <div class="mt-[3.5rem] ml-[-7.5rem]">{{ person.text }}</div>
      </div> 
                    </section>
                </div>
                <div class="flex">
                <div><input placeholder="enter Comment" v-model="commentAdded" class="ml-2 mt-2 border rounded-full pl-2 w-[37rem]" /></div>
                <div><button type="button" @click="addComment()" class="ml-1 mt-2 rounded bg-indigo-500 px-2 py-1 text-xs font-semibold text-white shadow-sm hover:bg-indigo-400 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-500">Send</button>
            </div></div></div>
              </DialogPanel>
            </TransitionChild>
          </div>
        </div>
      </div>
    </Dialog>
  </TransitionRoot>
</template>

<script setup>
import { ref, defineProps } from "vue";
import {
  Dialog,
  DialogPanel,
  DialogTitle,
  TransitionChild,
  TransitionRoot,
} from "@headlessui/vue";
import { XMarkIcon } from "@heroicons/vue/24/outline";

const open = ref(false);
const commentAdded=ref('')

const props = defineProps({
  openSidebar: Boolean,
});

const comment = [
  {
    name: 'Leslie Alexander',
    text: 'comment 1',
    imageUrl:
      'https://images.unsplash.com/photo-1494790108377-be9c29b29330?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=facearea&facepad=2&w=256&h=256&q=80',
  },
 
]

const addComment =()=>{
console.log("hh");
}
</script>
