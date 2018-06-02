<template>
  <div>
    <div 
      @click="nodeClicked"
      :style="{'margin-left': `${depth * 20}px`}"
      class="node"
    >
      <span 
        v-if="hasChildren"
        class="type"
      >{{expanded ? '&#9660;' : '&#9658;'}}</span>
      <span class="type" v-else>&#9671;</span>
      <span 
        :style="getStyle(node)">{{node.name}}</span>
    </div>
    <TreeBrowser 
      v-if="expanded"
      v-for="child in node.children"
      :key="child.name"
      :node="child"
      :depth="depth + 1"
      @onClick="(node) => $emit('onClick', node)"
    />
  </div>
</template>

<script>
import * as ColorHash from 'color-hash';
const colorHash = new ColorHash();

export default {
  name: 'TreeBrowser',
  props: {
    node: Object,
    depth: {
      type: Number,
      default: 0,
    }
  },
  data() {
    return {
      expanded: false,
    }
  },
  methods: {
    nodeClicked() {
      this.expanded = !this.expanded;
      if (!this.hasChildren) {
        this.$emit('onClick', this.node);
      }
    },
    getStyle(node) {
      let color = 'red';
      if (!node.children) {
        color = colorHash.hex(node.name.split('.')[1]);
      }
      return {
        color,
      }
    }
  },
  computed: {
    hasChildren() {
      return this.node.children;
    }
  }
}
</script>

<style scoped>
.node {
  text-align: left;
  font-size: 18px;
}

.type {
  margin-right: 10px;
}
</style>
