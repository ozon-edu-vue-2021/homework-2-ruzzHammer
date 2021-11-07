<template>
  <div id="app">
    <path-display :path="path"/>
    <ul class="tree">
      <tree-item
        :item="treeData"
        @path="getPath($event)"
        currentPath="/"
        :displayedPath="path"
      ></tree-item>
    </ul>
  </div>
</template>

<script>
import TreeData from '../public/static/node_modules.json'
import TreeItem from './components/TreeItem.vue'
import PathDisplay from './components/PathDisplay.vue'
export default {
  name: 'App',
  components: {
    TreeItem,
    PathDisplay
  },
  data() {
    return {
      treeData: TreeData,
      path: '/'
    }
  },
  provide() {
    return {
      getPath: this.getPath,
    }
  },
  methods: {
    getPath(path) {
      this.path = path
    }
  },
  mounted() {
    window.addEventListener('keyup', e => {
      if (this.$el.querySelector('.item__content ul') === null && (e.key === 'ArrowDown' || e.key === 'ArrowUp')) {
        this.$el.querySelector('.item__main').focus();
      }
    })
  }
}
</script>

<style>
body {
  margin: 0;
  padding: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #003780;
  margin: 0 auto;
}
*, *::before, *::after {
  box-sizing: border-box;
}
.item {
  cursor: pointer;
}
.bold {
  font-weight: bold;
}
.icon {
  fill: #003780;
}
p {
  margin: 0 0 5px;
}
ul, li {
  list-style-type: none;
  margin: 0;
  padding: 0;
}

ul {
  padding-left: 1em;
}

.tree {
  margin: 10px auto;
  padding: 0 10px;
  width: fit-content;
}

</style>
