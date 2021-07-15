<template>
  <input
    type="text"
    placeholder="Digite aqui..."
    v-model="state.search"
    @keyup="$emit(`search`, state.search)"
  >
</template>

<script>
import { reactive } from '@vue/reactivity'
export default {
  emits: ['search'],
  setup (_, { emit }) {
    const state = reactive({
      search: ''
    })
    return { state, emit }
  }
}
</script>

<style scoped>
input {
  width: 300px;
  padding: 5px 15px;
  border-radius: 15px;
  outline: none;
}
</style>