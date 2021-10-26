<template>
    <li class="item">
      <div
        @click="toggle">
            {{ item.name }}
        <span v-if="isFolder">[{{ isOpen ? '-' : '+' }}]</span>
      </div>
      <ul v-show="isOpen" v-if="isFolder">
        <tree-item
          class="item"
          v-for="(child, index) in item.contents"
          :key="index"
          :item="child"
        ></tree-item>
      </ul>
    </li>
</template>

<script>
export default {
    name: 'TreeItem',
     props: {
      item: Object
    },
    data() {
      return {
        isOpen: false
      };
    },
    computed: {
      isFolder() {
        return this.item.type === 'directory'
      }
    },
    methods: {
      toggle() {
        if (this.isFolder) {
          this.isOpen = !this.isOpen;
        }
      },
    }
}
</script>

<style>

</style>