<template>
  <Search @search="searchProps" />
  <Checked :dados="state.dados" />
</template>

<script>
import jsonFile from '../../json/atividade.json'
import Search from '../../components/Search'
import Checked from '../../components/Checked'
import { reactive } from '@vue/reactivity'
export default {
  name: 'App',
  components: {
    Search,
    Checked
  },
  setup () {
    const state = reactive({
      dados: [],
      search: ''
    })
    function removeAcento (text) {
      text = text.toLowerCase();
      text = text.replace(new RegExp('[ÁÀÂÃ]', 'gi'), 'a');
      text = text.replace(new RegExp('[ÉÈÊ]', 'gi'), 'e');
      text = text.replace(new RegExp('[ÍÌÎ]', 'gi'), 'i');
      text = text.replace(new RegExp('[ÓÒÔÕ]', 'gi'), 'o');
      text = text.replace(new RegExp('[ÚÙÛ]', 'gi'), 'u');
      text = text.replace(new RegExp('[Ç]', 'gi'), 'c');
      return text;
    }
    getData()
    function getData () {
      if (state.search.length > 0) {
        const jsonObject = jsonFile.filters[0].filters[0].filterOptions
        const noAccent = removeAcento(state.search)
        for (let item in jsonObject) {
          const fristItem = jsonObject[item].label.split(' ')
          const searchWithAccent = fristItem[0].toLowerCase().indexOf(state.search)
          const searchNoAccent = removeAcento(jsonObject[item].label.toLowerCase()).indexOf(noAccent)
          if (searchWithAccent !== -1) {
            if (state.dados.length >= 20) {
              state.dados.unshift(jsonObject[item])
              state.dados.pop()
            }
            if (state.dados.length < 20) state.dados.push(jsonObject[item])
          }
          if (searchNoAccent !== -1 && state.dados.length < 20) {
            state.dados.push(jsonObject[item])
          }
        }
      }
    }
    function searchProps (info) {
      state.dados = []
      state.search = info
      getData()
    }

    return { state, searchProps }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
