<template>
  <div class="map-container">
    <Panel @update-marks="updateMarks" />
    <l-map class="map" ref="myMap" :zoom="zoom" :center="center" :markerZoomAnimation="true">
      <l-tile-layer :url="url"></l-tile-layer>
      <l-marker v-for="(mark, index) in markers" :key="index" :lat-lng="mark" />
      <l-circle
        v-if="circle.center"
        :lat-lng="circle.center"
        :radius="circle.radius"
        :color="circle.color"
      />
    </l-map>
  </div>
</template>

<script>
import L from "leaflet";
import { LMap, LTileLayer, LMarker, LCircle } from "vue2-leaflet";
import Panel from "@/components/Panel.vue";

export default {
  name: "MyAwesomeMap",
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LCircle,
    Panel,
  },
  data() {
    return {
      url: "https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}{r}.png",
      zoom: 3,
      center: L.latLng(47.41322, -1.219482),
      markers: [],
      circle: {
        center: null,
        radius: 0,
        color: "red",
      },
    };
  },
  methods: {
    updateMarks(data) {
      let lat = data.parameters.latitude;
      let lng = data.parameters.longitude;
      let rad = data.parameters.radius;

      this.markers = data.coords;
      this.circle.radius = rad * 1000;
      this.circle.center = [lat, lng];
      // Vue Leaflet library doesn't work properly when updating these two,
      // so I change the properties of the map directly https://github.com/vue-leaflet/Vue2Leaflet/issues/170
      this.$refs.myMap.setZoom(10);
      this.$refs.myMap.setCenter([lat, lng]);
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
