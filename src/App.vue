<script setup lang="ts">


import HorizontalListItem from "./components/HorizontalListItem.vue";
import {useScroll, useVirtualList} from "@vueuse/core";
import {shallowRef} from "vue";


const initList = shallowRef(Array(110).fill([]))

const { list, containerProps, wrapperProps } = useVirtualList(
    initList,
    {
      itemHeight: 20,
    },
)

const { isScrolling } = useScroll(containerProps.ref)

</script>

<template>
  <header class="sticky bg-white top-0 py-5 border-b-2 z-10">
   <div class="pl-2">
     list for omg
   </div>
  </header>
  <main
      v-bind="containerProps"
      class="h-full flex justify-center mt-2">
   <div
       v-bind="wrapperProps"
       class="border rounded-md w-full h-full md:w-1/2 p-2">
     <HorizontalListItem
         class="border rounded-md mb-2 h-20 flex items-center justify-center overflow-x-scroll"
         v-for="(_, index) in list"
         :key="index"
         :is-scrolling="isScrolling"
     />
   </div>
  </main>
</template>

