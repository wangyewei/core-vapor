<script setup lang="ts">
import {
  ref,
  computed,
  onMounted,
  onBeforeMount,
  getCurrentInstance,
  onBeforeUpdate,
  onUpdated,
} from 'vue/vapor'

const instance = getCurrentInstance()!
const count = ref(1)
const double = computed(() => count.value * 2)
const html = computed(() => `<button>HTML! ${count.value}</button>`)

const arr = ref([1, 2, 3])
const obj = ref({ a: 1, b: 2, c: 3 })

const inc = () => count.value++
const dec = () => count.value--

onBeforeMount(() => {
  console.log('onBeforeMount', instance.isMounted)
})
onMounted(() => {
  console.log('onMounted', instance.isMounted)
})
onMounted(() => {
  setTimeout(() => {
    count.value++
  }, 1000)
})

onBeforeUpdate(() => {
  console.log('before updated')
})
onUpdated(() => {
  console.log('updated')
})

const log = (arg: any) => {
  console.log('callback in render effect')
  return arg
}

const objChange = () => (obj.value = { e: 4, f: 5, g: 6 })
const arrChange = () => (arr.value = [4, 5, 6])
</script>

<template>
  <div>
    <h1 class="red">Counter</h1>
    <div>The number is {{ log(count) }}.</div>
    <div>{{ count }} * 2 = {{ double }}</div>
    <div style="display: flex; gap: 8px">
      <button @click="inc">inc</button>
      <button @click="dec">dec</button>
    </div>
    <div v-html="html" />
    <div v-text="html" />
    <div v-once>once: {{ count }}</div>
    <div v-pre>{{ count }}</div>
    <div v-cloak>{{ count }}</div>

    <button @click="arrChange">change arr</button>
    <button @click="objChange">change obj</button>

    <div>arr:</div>
    <div v-for="(item, index) in arr">{{ item }}-{{ index }}</div>

    <div>obj:</div>
    <div v-for="(item, index) in obj">{{ item }}-{{ index }}</div>
  </div>
</template>

<style>
.red {
  color: red;
}

html {
  padding: 10px;
}
</style>
