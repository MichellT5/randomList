<template>
  <div class="p-5">
    <div class="container mx-auto grid gap-5">
      <h1 class="text-xl">Randomizer</h1>
      <hr>
      <div class="grid gap-2">
        <div class="grid gap-2">
          <label>Datos:</label>
          <textarea rows="10" class="rounded border text-black px-2 py-1 font-mono" v-model="datos"></textarea>
        </div>
        <div class="flex gap-2">
          <button class="rounded-full p-2 bg-blue-700 text-white w-20" @click="doRandomList">
            Mix
          </button>
          <button class="rounded-full p-2 bg-red-700 text-white w-20" @click="removeFirst">
            Remove
          </button>
          <button class="rounded-full p-2 bg-green-700 text-white w-20" @click="popOne">
            Pop One
          </button>
          <button class="rounded-full p-2 bg-amber-700 text-white w-20" @click="replace">
            Replace
          </button>
        </div>
      </div>
      <div class="grid gap-2">
        Showing {{ list.length }} elements
        <div v-for="el in list" :key="el">
          <a v-if="isLink(el)" :href="el" class="text-blue-600 dark:text-blue-500" target="_blank">{{ el }}</a>
          <span v-else>{{ el }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { reactive, ref } from 'vue';

const list = reactive<string[]>([])

const datos = ref("");

const removeFirst = function () {
  const el = list.shift()
  if (el) {
    // datos.value = datos.value.split("\n").filter(e => e != el).join("\n");
    return el;
  }
}

const popOne = () => {
  const el = removeFirst();
  if (el) {
    if (isLink(el)) window.open(el, '_blank')
  }
}

const isLink = (link: string) => {
  return link.match(/^http/)
}

const replace = () => {
  datos.value = list.join("\n")
}

const doRandomList = () => {
  list.length = 0;
  const templist = datos.value.split("\n")
    .filter(el => el)
    .map(el => ({ el, i: Math.random() }))
    .sort(({ i: A }, { i: B }) => A - B)
    .map(({ el }) => el)
  list.push(...templist)
}

</script>