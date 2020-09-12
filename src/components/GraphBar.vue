<template>
  <div class="graph-bar">
    <span
      class="bar"
      :style="{'width': (value.count * 300 / barMaxPixels) + 'px'}"
    >
    {{value.count}}
    </span>
    <span class="bar-value">{{value.value}}</span>
  </div>
</template>

<script>
export default {
  name: 'GraphBar',
  props: [
    'barMaxPixels',
    'value'
  ]
}
</script>

<style>

.bar {
  margin-right: 10px;
  background-color: var(--lightRed);
  color: white;
  border-top-right-radius: 6px;
  border-bottom-right-radius: 6px;
  font-size: 0.7rem;
  height: 70%;
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: hidden;
  text-overflow: ellipsis;
}

.bar-value {
  font-size: 1rem;
  color: var(--grey);
}

.graph-bar {
  height: 1.3rem;
  display: flex;
  align-items: center;
}

</style>