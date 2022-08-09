<script setup lang="ts">
function onClick() {
  alert('click!')
}
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