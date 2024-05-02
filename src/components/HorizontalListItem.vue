<template>
  <div ref="target"
       class="flex w-full">
    <div
        v-for="(number, index) in list"
        :key="index"
        class="cell border rounded-md w-[60px] min-w-[60px] h-[60px] mr-2 flex items-center justify-center cursor-pointer active:bg-gray-200 hover:scale-80"
    >
      {{ number }}
    </div>
  </div>
</template>

<script setup lang="ts">
import {ref} from 'vue';
import {useElementVisibility, useIntervalFn} from "@vueuse/core";


const props = defineProps({
  isScrolling: {
    type: Boolean,
    default: false
  }
})

const list = ref([...Array(11)].map(() => Math.floor(Math.random() * 10) + 1))

const target = ref<HTMLElement | null>(null)

const targetIsVisible = useElementVisibility(target)


const updateList = () =>  {
  if(!targetIsVisible.value || props.isScrolling) {
    return
  }
  const cells = [...target.value!.querySelectorAll('.cell')]
  const start = cells.findIndex((cell) => cell.getBoundingClientRect().right >= target.value!.getBoundingClientRect().left)
  let end = cells.findIndex((cell) => cell.getBoundingClientRect().left >= target.value!.getBoundingClientRect().right)
  if (end === -1) {
    end = cells.length - 1
  }
  const randomPosition = start + Math.floor(Math.random() * (end - start))
  list.value[randomPosition] = Math.floor(Math.random() * 10) + 1
}

useIntervalFn(updateList, 1000)
</script>