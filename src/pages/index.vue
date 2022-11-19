<script setup lang="ts">
let text = $ref('')
let editText = $ref('')
const todos: string[] = $ref([])

// const router = useRouter()
const addTodo = () => {
  if (!text)
    return

  todos.push(text)
  text = ''
  // if (text)
  //   router.push(`/hi/${encodeURIComponent(text)}`)
}
const updateTodo = (idx: number) => {
  todos[idx] = editText
  editText = ''
}
const removeTodo = (idx: number) => {
  todos.splice(idx, 1)
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

    <div class="flex flex-col items-center">
      <div
        v-for="(i, idx) in todos" :key="idx"
        class=" dark:bg-gray-800 w-2/6 flex flex-row gap-x-5 border border-color-gray-800 dark:border-color-gray-700 py-sm rounded my-xs px-sm justify-center items-center"
      >
        <div>
          {{ idx + 1 }}
        </div>
        <div class="w-5/6">
          <div v-if="!editText.length" @click="editText = i">
            {{ i }}
          </div>
          <div v-else>
            <input
              id="input"
              v-model="editText"
              placeholder="What's on your mind?"
              type="text"
              autocomplete="false"
              p="x-4 y-2"
              w="250px"
              text="center"
              bg="transparent"
              border=" bottom"
              outline="none active:none"
              @keydown.enter="updateTodo(idx)"
            >
          </div>
        </div>
        <div class="flex flex-row items-center gap-5">
          <div v-if="editText.length" class="i-carbon-edit bg-green cursor-pointer hover:bg-green-700" @click="updateTodo(idx)" />
          <div class="i-carbon:trash-can bg-red cursor-pointer hover:bg-red-700" @click="removeTodo(idx)" />
        </div>
      </div>
    </div>
  </div>
</template>
