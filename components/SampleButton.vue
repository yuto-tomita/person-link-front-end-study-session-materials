<script setup lang="ts">
function onClick() {
  alert('click!')
}
// const a: number = '300'
// const b: number = '400'
// const c: string = '500'

// const sum: number = a + b // 3
// const sum = a + b + c // 700500
// type union = 'a' | 'b'
// const a: union = 'b'
// type Obj = {
//   readonly foo: string
//   readonly bar: number
// }
// // let obj: Obj = Object.create({})
// const obj: Obj = {
//   foo: '',
//   bar: 0
// }
// const str = 'a'
// str = 'i'
// obj.bar = 1

// const ary: string[] = ['a', 'i', 'u']
// console.log(ary[3]) //undefined

// プロパティアクセス
// console.log(obj.foo) // 'aaa' string
// console.log(obj.bar) // 0 number
// obj.foo = 'iii'


// const test: Obj[] = [
//   {
//     foo: 'aaa',
//     bar: 0
//   },
// ]
// const test: string[] = ['a', 'b', 0]
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