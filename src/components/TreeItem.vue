<template>
    <li class="item">
        <div 
        @click.stop="handleSelection()"
        @keypress.enter="handleSelection()"
        @keydown="handleKeys"
        tabindex="-1"
        :class="[{
            selected: displayedPath === itemPath,
        },
        'item__main']">
            <component 
            :is="icon"
            v-bind="{active: isOpen}"
            ></component>
            <p>{{ item.name }}</p>
        </div>
        <div class="item__content">
            <ul v-if="isFolder && isOpen">
                <tree-item
                v-for="(child, index) in item.contents"
                @path="getPath($event)"
                :key="index"
                :item="child"
                :currentPath="itemPath"
                :displayedPath="displayedPath"
                ></tree-item>
            </ul>
        </div>
    </li>
</template>

<script>
import IconDirectory from './Icons/IconDirectory.vue'
import IconFile from './Icons/IconFile.vue'
import IconLink from './Icons/IconLink.vue'
export default {
    name: 'TreeItem',
    components: {IconDirectory, IconFile, IconLink},
    props: {
      item: Object,
      currentPath: {
          type: String,
          default: ''
      },
      displayedPath: {
          type: String,
          default: ''
      },
    },
    data() {
      return {
        isOpen: false,
        isSelected: false,
      }
    },
    inject: ['getPath'],
    computed: {
      isFolder() {
        return this.item.type === 'directory'
      },
      icon() {
        return `Icon${this.item.type[0].toUpperCase()}${this.item.type.slice(1)}`
      },
      itemPath() {
          return this.currentPath + this.item.name + (this.isFolder ? '/' : ''); 
      }
    },
    methods: {
      toggle() {
        if (this.isFolder) {
            this.isOpen = !this.isOpen;
        }
      },
      passPath() {
          this.$emit('path', this.itemPath)
      },
      handleSelection() {
          this.passPath();
          this.toggle();
          this.$el.querySelector('.item__main').focus();
      },
      goDown(item) {
        let nextItem = item.querySelector('.item__content li.item') || item.nextElementSibling;
        if (nextItem !== null) {
            nextItem.querySelector('.item__main').focus();
        } else {
            while (item.nextElementSibling === null) {
                if (item.closest('.item__content') === null) {
                    return false;
                } else {
                    item = item.closest('.item__content').closest('li.item');
                }
            }
            nextItem = item.nextElementSibling || item.querySelector('.item__content li.item');
            nextItem.querySelector('.item__main').focus();
        }
      }, 
      goUp(item) {
        let prevAncestorChildren = item.previousElementSibling?.querySelectorAll('li.item');
        let prevAncestorLastChild = prevAncestorChildren !== undefined ? prevAncestorChildren[prevAncestorChildren.length - 1] : undefined;
        let prevItem = prevAncestorLastChild || item.previousElementSibling || item.closest('.item__content')?.closest('li.item');
        if (prevItem !== undefined) {
            prevItem.querySelector('.item__main').focus();
        }
      },
      handleKeys(event) {
          let item = event.target.closest('li.item');
          if (event.key === 'ArrowDown') {
              event.preventDefault();
              this.goDown(item)
          } else if (event.key === 'ArrowUp') {
              event.preventDefault();
              this.goUp(item)
          }
      }
    },
}
</script>

<style scoped>
.item__main {
    display: flex;
    align-items: center;
    gap: 15px;
    padding: 2px;
}
.item__main p {
    margin: 0;
}
.item__main.selected {
    background: #3b3b3b;
}
.item__main.selected p {
    color: #fff;
}
.item__main.selected svg {
    fill: #fff;
}
.item__main:focus {
    outline: 1px dashed rgb(47, 0, 218);
}
</style>
