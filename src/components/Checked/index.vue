<template>
  <div>
    <form class="items">
      <label
        v-for="(data, key) in state.dados"
        :key="key"
      >
        <div class="line">
          <p>{{data.label}}</p>
          <input
            :checked="data.checked"
            @click="checked(data)"
            type="checkbox"
            :name="data.label"
            :id="key"
          >
        </div>
      </label>
    </form>
  </div>
</template>

<script>
import { reactive } from '@vue/reactivity'
import { watch } from '@vue/runtime-core'
export default {
  props: {
    dados: Array
  },
  setup (props) {
    const state = reactive({
      dados: []
    })
    watch(props, () => {
      if (!localStorage.dataChecked) localStorage.dataChecked = ''
      state.dados = props.dados.map(element => {
        localStorage.dataChecked.split(',').forEach(el => {
          if (el === element.value) {
            element.checked = true
            return element
          }
        })
        return element
      })
    })
    function checked (data) {
      const localStorageDataChecked = localStorage.dataChecked
      if (!localStorageDataChecked) {
        data.checked = true
        localStorage.setItem('dataChecked', JSON.stringify(Number(data.value)))
      } else if (localStorage.dataChecked.indexOf(data.value) === -1) {
        localStorage.dataChecked = `${localStorage.dataChecked},${data.value}`
        data.checked = true
      } else {
        data.checked = false
        let arryDataLocalStorage = localStorage.dataChecked.split(',')
        arryDataLocalStorage.splice(arryDataLocalStorage.indexOf(data.value), 1)
        localStorage.dataChecked = arryDataLocalStorage.join(',')
      }
    }
    return { state, checked }
  }
}
</script>

<style scoped>
.line {
  display: flex;
  flex-direction: row-reverse;
  align-items: center;
  margin-top: 10px;
  justify-content: flex-end;
}
.line input {
  height: 15px;
  width: 15px;
  margin-left: -20px;
  position: absolute;
  cursor: pointer;
}
.items {
  margin: 30px auto;
  max-width: 500px;
  white-space: nowrap;
}
.items p {
  overflow: hidden;
  text-overflow: ellipsis;
}
</style>