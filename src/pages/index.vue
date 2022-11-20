<script setup lang="ts">
import type { Nullable } from '@antfu/utils'

let text = $ref('')
interface Item {
  idx: number
  content: string
}
let editItem: Nullable<Item> = $ref(null)
let todoHeader = $ref('Todos')
const todos: string[] = $ref([])

const addTodo = () => {
  if (!text)
    return
  todos.push(text)
  text = ''
}

const updateTodo = () => {
  const editTextField = document.getElementById('editTextField')
  if (editItem && editTextField) {
    let editText = editTextField.innerText
    if (editText[editText.length - 1] === '\n')
      editText = editText.slice(0, -1)
    todos[editItem.idx] = editText
    editItem = null
  }
}
const removeTodo = (idx: number) => {
  todos.splice(idx, 1)
  if (editItem && editItem.idx === idx)
    editItem = null
}
const updateTodoHeader = () => {
  const headerField = document.getElementById('headerEditField')
  if (headerField) {
    let editText = headerField.innerText
    if (editText[editText.length - 1] === '\n')
      editText = editText.slice(0, -1)
    todoHeader = editText
    editItem = null
  }
}
</script>

<template>
  <div>
    <div i-carbon-campsite text-4xl inline-block />
    <p>
      <a rel="noreferrer" href="https://github.com/antfu/vitesse-lite" target="_blank">
        Todo - v2
      </a>
    </p>
    <p>
      <em text-sm op75>Todos - Counters - Trackers</em>
    </p>

    <div py-4 />

    <div relative class="w-2/6 overflow-y-scroll max-h-96" border="~ border-color-dark rounded">
      <div sticky top-0 dark:bg-dark bg-white px-xs pt-xs z-10>
        <div
          id="headerEditField"
          text-2xl
          flex
          flex-row
          justify-start
          contenteditable="true"
          @keydown.enter="updateTodoHeader"
        >
          {{ todoHeader }}
        </div>
        <div>
          <input
            id="input"
            v-model="text"
            placeholder="What's on your mind?"
            type="text"
            autocomplete="false"
            p="x-4 y-2"
            w="250px"
            text="center"
            bg="transparent"
            border="~ rounded gray-200 dark:gray-700"
            outline="none active:none"
            @keydown.enter="addTodo"
          >
          <button
            class="m-3 text-sm btn"
            :disabled="!text"
            @click="addTodo"
          >
            Add
          </button>
        </div>
      </div>
      <div pa-sm>
        <div
          v-for="(i, idx) in todos" :key="idx"
          class=" dark:bg-gray-800 flex flex-row gap-x-5 border border-color-gray-800 dark:border-color-gray-700 py-sm rounded my-xs px-sm justify-center items-center"
        >
          <div>
            {{ idx + 1 }}
          </div>
          <div class="w-4/6">
            <div
              v-if="editItem && editItem.idx === idx"
              border="~ rounded transparent"
              hover="border-b-dark dark:border-b-white"
            >
              <div
                id="editTextField"
                ref="editTest"
                h-max
                p-2
                outline-none
                rounded
                contenteditable="true"
                @keydown.enter="updateTodo"
              >
                {{ editItem.content }}
              </div>
            </div>
            <div v-else break-all @click="editItem = { idx, content: i }">
              {{ i }}
            </div>
          </div>
          <div class="flex flex-row items-center gap-5">
            <div v-if="editItem && editItem.idx === idx" class="i-carbon-edit bg-green-700 cursor-pointer hover:bg-green" @click="updateTodo" />
            <div class="i-carbon:trash-can bg-red-700 cursor-pointer hover:bg-red" @click="removeTodo(idx)" />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
