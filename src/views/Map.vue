<template>
  <div class="map-container">
    <Panel @update-marks="updateMarks" />
    <l-map class="map" ref="myMap" :zoom="zoom" :center="center">
      <l-tile-layer :url="url"></l-tile-layer>
      <l-marker v-for="(mark, index) in markers" :key="index" :lat-lng="mark" />
    </l-map>
  </div>
</template>

<script>
import L from "leaflet";
import { LMap, LTileLayer, LMarker } from "vue2-leaflet";
import Panel from "@/components/Panel.vue";

export default {
  name: "MyAwesomeMap",
  components: {
    LMap,
    LTileLayer,
    LMarker,
    Panel,
  },
  data() {
    return {
      url: "https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}{r}.png",
      zoom: 3,
      center: L.latLng(47.41322, -1.219482),
      markers: [],
    };
  },
  methods: {
    updateMarks(newMarks) {
      this.markers = newMarks;
    },
  },
};
</script>

<style scoped>
.map-container {
  width: 100%;
  min-height: 100vh;
  display: flex;
  align-items: center;
}

.map {
  width: 100%;
  min-height: 100vh;
}
</style>
