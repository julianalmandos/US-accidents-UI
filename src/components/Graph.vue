<template>
  <div class="graph">
    <h3 id="graph-title" class="description">
      <slot name="title"></slot>
    </h3>
    <div>
      <GraphBar v-for="value in values" :key="value.value" :value="value" :barMaxPixels="barMaxPixels"/>
    </div>
  </div>
</template>

<script>
import GraphBar from '@/components/GraphBar.vue';

export default {
  name: 'Graph',
  components: {
    GraphBar
  },
  props: [
    'values'
  ],
  computed: {
    barMaxPixels() {
      return this.getMaxValue(this.values);
    }
  },
  methods: {
    getMaxValue(values) {
      return values.map(value => value.count).sort((a, b) => b-a)[0];
    }
  }
}
</script>

<style>
.graph {
  margin-top: 10px;
}

#graph-title {
  margin: 0;
}
</style>