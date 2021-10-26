<template>
  <div id="app">
    <tree-item
      :item="treeData"
    ></tree-item>
  </div>
</template>

<script>
import treeData from '../public/static/node_modules.json'
import treeItem from './components/TreeItem.vue'
export default {
  name: 'App',
  data() {
    return {
      treeData: treeData
    }
  },
  components: {
    treeItem
  },
  methods: {
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
.item {
  cursor: pointer;
}
.bold {
  font-weight: bold;
}


</style>
