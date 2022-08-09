<script setup lang="ts">
function onClick() {
  alert('click!')
}
// const a: number = '300'
// const b: number = '400'
// const c: string = '500'

// const sum: number = a + b // 3
// const sum = a + b + c // 300400500
// type union = 'a' | 'b'
// const a: union = 'a'
</script>

<template>
  <button
    type="button"
    class="border border-slate-900 p-1"
    @click="onClick"
  >
    click me
  </button>
</template>