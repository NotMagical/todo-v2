<script setup lang="ts">
import type { Nullable } from '@antfu/utils'
import { v4 as uuidv4 } from 'uuid'

let text = $ref('')
interface Item {
  id: string
  content: string
  completed: boolean
}
let editItem: Nullable<Item> = $ref(null)
let todoHeader = $ref('Todos')
const todos: Item[] = $ref([])

const addTodo = () => {
  if (!text)
    return
  todos.push({ id: uuidv4(), content: text, completed: false })
  text = ''
}

const updateTodo = (idx: number) => {
  const editTextField = document.getElementById('editTextField')
  if (editItem && editTextField) {
    let editText = editTextField.innerText
    if (editText[editText.length - 1] === '\n')
      editText = editText.slice(0, -1)
    todos[idx].content = editText
    editItem = null
  }
}
const removeTodo = (idx: number) => {
  todos.splice(idx, 1)
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
          :class="i.completed ? 'opacity-40' : undefined"
          hover="bg-gray-100 dark:bg-gray-7"
        >
          <div class="flex flex-row items-center gap-2">
            <div hover=" bg-green i-carbon:checkmark-filled" :class="i.completed ? 'i-carbon:checkmark-filled bg-green' : 'i-carbon:checkmark'" @click="i.completed = !i.completed" />
            {{ idx + 1 }}
          </div>
          <div class="w-4/6">
            <div
              v-if="editItem && editItem.id === i.id"
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
                @keydown.enter="updateTodo(idx)"
              >
                {{ editItem.content }}
              </div>
            </div>
            <div v-else :class="i.completed ? 'line-through' : undefined" break-all @click="editItem = i">
              {{ i.content }}
            </div>
          </div>
          <div class="flex flex-row items-center gap-5">
            <div v-if="editItem && editItem.id === i.id" class="i-carbon-edit bg-green-700 cursor-pointer hover:bg-green" @click="updateTodo(idx)" />
            <div class="i-carbon:trash-can bg-red-700 cursor-pointer hover:bg-red" @click="removeTodo(idx)" />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
