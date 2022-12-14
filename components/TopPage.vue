<!-- setupにする利点
  - setup関数で必要なプロパティのreturnがいらない
  - componentのimport時にcomponentsプロパティが必要ない
  definePropsでpropsの定義ができる。interfaceの定義でpropsが登録できる等々たくさん -->
<script setup lang="ts">

// setup() {
//   ...

//   return {
//     ...
//   }
// }
// props: {
//   msg: {
//     type: String,
//     required: true
//   }
// }
import { reactive, ref, computed, ErrorCodes } from 'vue'
import Input from './ui/Input/InputUi.vue'
import Button from './ui/Button/ButtonUi.vue'
import TodoList from './todo/List/TodoList.vue'
import {
  Todo,
  TodoForm,
  Status,
  TodoErrorMessage
} from './todo/types'

// reactiveを使用することでリアクティブに動作するようになる。refと違う点はvue2のdataプロパティのように記述できる点と、分割代入をすることでリアクティブ性が失われる点、reactiveに動作させるための内部ロジックが違う
// コンポーネントのネストでもリアクティブ性が失われるので、扱いが難しい
const todoForm = reactive<TodoForm>({
  todo: 'aaa',
  status: Status.PROGRESS
})

// refを使用した値はリアクティブに動作するようになる。`.value`にアクセスすることで`todos`の値にアクセスすることができる。
const todos = ref<Todo[]>([])
const error = ref<TodoErrorMessage>({
  todo: ''
})
// ジェネリクスに型を指定することができる
// 当然stringで指定しているため、string以外の型を代入することができない
const test = ref<string>('aaaa')

// 型を記述しなくても簡単な型ならtypescriptの型推論が働いてstring型が自動的に割り振られる
// const test = ref('')

// test.value = 1
// test.value = {}
// test.value = 'sample'

// 第一引数に子コンポーネント側のemitの第二引数に指定した、受け取ることができる
const deleteTodo = (event: number) => {
  todos.value = todos.value.filter(val => val.id !== event)
}

/** 投稿フォームが空の状態では投稿できないようにする */
const formValidate = () => {
  if (!todoForm.todo) {
    error.value.todo = '内容を入力してください'

    // 分割代入を使って条件分岐をするために、オブジェクト形式で値を返却する
    return { invalid: true }
  } else {
    return { invalid: false }
  }
}

/** ユーザーが入力したTodoをrefに保存する */
const saveTodo = () => {
  // 分割代入(https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)
  // formValidateから返却されるオブジェクトの値を取り出して、別の変数として利用できる
  const { invalid } = formValidate()

  if (!invalid) {
    // refでtodosの値を管理しているため、todosの変更を検知したら連動してtodosの内容が画面に描画される
    todos.value.push({
      id: todos.value.length + 1,
      todo: todoForm.todo,
      status: todoForm.status,
    })
  }

  // 投稿したらフォームを空にする
  todoForm.todo = ''
}

const switchTodoStatus = (event: number) => {
  // findIndexでテスト関数に一致したオブジェクトのindex(添字)を取得する
  const todoIndex = todos.value.findIndex(val => val.id === event)

  // チェックアイコンをクリックしたら、statusを反転させる
  if (todos.value[todoIndex].status === Status.COMPLETE) {
    todos.value[todoIndex] = {
      // spread構文(...)を使用すると、オブジェクトや配列の値を展開させることができる。
      // 同一プロパティがあった場合、そのプロパティも更新する特性を持っている
      ...todos.value[todoIndex],
      status: Status.PROGRESS
    }

    // これは上記のspread構文を使用しなかった場合の記述方法

    // todos.value[todoIndex] = {
    //   deadline: todos.value[todoIndex].deadline,
    //   id: todos.value[todoIndex].id,
    //   todo: todos.value[todoIndex].todo,
    //   status: Status.PROGRESS
    // }
  } else {
    todos.value[todoIndex] = {
      ...todos.value[todoIndex],
      status: Status.COMPLETE
    }
  }
}

const updateTodoValue = (event: { todo: string, id: number }) => {
  // findIndexでテスト関数に一致したオブジェクトのindex(添字)を取得する
  const todoIndex = todos.value.findIndex(val => val.id === event.id)

  todos.value[todoIndex] = {
    ...todos.value[todoIndex],
    todo: event.todo
  }
}

const methodsRumdom = () => {
  return Math.random()
}

const computedRumdom = computed(() => Math.random())

// const getErrorMessage = () => {
//   return error.value.todo
// }
const getErrorMessage = computed(() => {
  return error.value.todo
})
</script>

<!-- templateに記述されているHTMLはVueのRender関数によってVNode(Virtual Node(仮想DOM))を生成し、DOMに変換される -->
<template>
  <div class="flex w-11/12 items-center justify-center">
    <div class="mr-2 flex w-4/12 whitespace-nowrap">
      <span class="mr-1">やること</span>

      <!-- 子コンポーネントでmodelValueという値を受け取るように記述しているため、親コンポーネントでv-modelで管理できるようになる -->
      <!-- 親コンポーネントでv-model管理のメリットとして、データフローが追いやすくなる、データ管理の容易さ、簡潔にかけると言ったメリットがある -->
      <!-- :model-value="OO" @update:modelValue="OO" -->
      <Input
        v-model="todoForm.todo"
        class="w-full"
        :error-message="getErrorMessage"
      />
    </div>
    <!-- 双方向データバインディングの例(Inputに入力されている値が連動して画面に表示されている) -->
    <!-- {{ todoForm.todo }} -->

    <div class="ml-2">
      <!-- @* はv-on:* のショートハンド記法　子コンポーネント側でemitしたイベントを発火させることができる -->
      <!-- 子コンポーネントでの、×アイコンをクリックした時に、TODOを消す処理を発火させる -->
      <Button @click="saveTodo">
        投稿
      </Button>
    </div>
  </div>
  <TodoList
    :todo-list="todos"
    @on-delete="deleteTodo"
    @on-complete="switchTodoStatus"
    @on-update="updateTodoValue"
  />
  {{ computedRumdom }}<br>
  {{ computedRumdom }}<br>
  {{ computedRumdom }}<br>
  {{ computedRumdom }}<br>
  <br>
  <br>
  {{ methodsRumdom() }}<br>
  {{ methodsRumdom() }}<br>
  {{ methodsRumdom() }}<br>
  {{ methodsRumdom() }}<br>
</template>


<style scoped lang="lang">
</style>

<!-- Vueでtemplateを書いていく上でのアンチパターン -->

<!-- ① -->
<!-- 危険性を理解しないでv-htmlを使用すること(XSS攻撃を起こされる可能性がある) -->

<!-- XSS攻撃とは： 攻撃者が悪意のあるスクリプトをアプリケーションに埋め込み、何も知らないユーザーにそのスクリプトを実行させ攻撃の標的となるWebサイトに誘導するという攻撃 -->
<!-- 仮にユーザーの入力内容をそのまま画面に表示するといった仕様を実装するとなった時に、v-htmlを使用すると悪意のあるスクリプトをそのままHTMLとして表示することになるため、攻撃される的になってしまう -->

<!-- ② -->
<!-- 複雑な式をマスタッシュ構文で実行させる -->

<!-- {{ isAdmin ? '管理者' : '一般ユーザー' }} ユーザーがAdminユーザーかどうかを表示しています。こういった式はcomputedを使用してバインドさせてほうがいい -->
<!-- なぜなら仮想DOMのコンセプトとなる宣言的UIの思想にはずれてしまうから。宣言的UIとはあるべき状態を記述してUIを構築するという考え方だが、上記の例はあるべき姿が想像しづらいですよね。 -->
<!-- そのためcomputedプロパティを使用して、式の意味を命名してあげると宣言的UIの思想に近づけるようにしましょう。マスタッシュ構文で記述するとデバッグがしづらいです。同じ式のマスタッシュ構文が複数記述されていると尚更です。 -->

<!-- JavaScript式を何でもかんでもMethodsプロパティで実行させる -->

<!-- 確かにComputedもMethodsも式が同じなら同じ値を返します。しかし、何でもかんでもMethodsで実行させるのではなく、computedを使用しましょう。使える場面ならばcomputedを極力使えるようにしましょう -->

<!-- computedは結果をキャッシュするという特性を持っています。一回実行して、算出された値が同じならば関数を再実行しないでそのまま値を返します。 -->

<!-- 対してMethodsは呼ばれるごとに関数を実行させてしまいます。これはパフォーマンス上良くないです。実行する必要がないのに実行させてるわけですから。 -->

<!-- <style scoped lang="scss"></style> -->