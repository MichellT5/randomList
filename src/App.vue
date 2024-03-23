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
        <div class="grid gap-2 grid-cols-2">
          <button class="rounded-full p-2 bg-blue-700 text-white" @click="initRandomList">
            Mix
          </button>
          <button v-if="list.length > 1" class="rounded-full p-2 bg-indigo-900 text-white" @click="remix">
            Remix
          </button>
          <button class="rounded-full p-2 bg-red-700 text-white" @click="removeFirst">
            Remove
          </button>
          <button class="rounded-full p-2 bg-green-700 text-white" @click="popOne">
            Pop One
          </button>
          <button class="rounded-full p-2 bg-amber-700 text-white" @click="replace">
            Replace
          </button>
          <button v-if="list.length > 0" class="rounded-full p-2 bg-slate-700 text-white" @click="empty">
            Empty
          </button>
        </div>
      </div>
      <div class="grid gap-2 w-full break-all">
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
import { reactive, ref } from 'vue'

const list = reactive<string[]>([])

const datos = ref("")

const removeFirst = function () {
  const el = list.shift()
  if (el) return el
}

const popOne = () => {
  const el = removeFirst()
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

const empty = () => {
  list.length = 0
}

const initRandomList = () => {
  list.length = 0
  const templist = mix(datos.value.split("\n")
    .filter(el => el))
  list.push(...templist)
}

const mix = <T>(list: T[]): T[] => {
  return list.map(el => ({ el, i: Math.random() }))
    .sort(({ i: A }, { i: B }) => A - B)
    .map(({ el }) => el)
}

const remix = () => {
  const templist = mix(list)
  list.length = 0
  list.push(...templist)
}

</script>