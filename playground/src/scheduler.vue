<script setup lang="ts">
import {
  onEffectCleanup,
  ref,
  watch,
  watchEffect,
  watchPostEffect,
  watchSyncEffect,
} from 'vue/vapor'

const source = ref(0)
const add = () => source.value++

watchPostEffect(() => {
  const current = source.value
  console.log('post', current)
  onEffectCleanup(() => console.log('cleanup post', current))
})

watchEffect(() => {
  const current = source.value
  console.log('pre', current)
  onEffectCleanup(() => console.log('cleanup pre', current))
})

watchSyncEffect(() => {
  const current = source.value
  console.log('sync', current)
  onEffectCleanup(() => console.log('cleanup sync', current))
})

watch(source, (value, oldValue) => {
  console.log('sync watch', value, 'oldValue:', oldValue)
  onEffectCleanup(() => console.log('cleanup sync watch', value))
})

const onUpdate = (arg: any) => {
  const current = source.value
  console.log('render', current)
  onEffectCleanup(() => console.log('cleanup render', current))
  return arg
}
</script>

<template>
  <div>
    <p>Please check the console</p>
    <div>
      <button @click="add">Add</button>
      |
      <span>{{ onUpdate(source) }}</span>
    </div>
  </div>
</template>

<style>
.red {
  color: red;
}

html {
  color-scheme: dark;
  background-color: #000;
  padding: 10px;
}
</style>
