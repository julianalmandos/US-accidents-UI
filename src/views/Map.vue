<template>
  <div class="map-container">
    <LoadingMarkers v-if="loadingMarkers" />
    <Panel @update-marks="updateMarks" @loading-markers="toggleLoadingMarkers" />
    <l-map class="map" ref="myMap" :zoom="zoom" :markerZoomAnimation="true">
      <l-tile-layer :url="url"></l-tile-layer>
      <l-marker v-for="(mark, index) in markers" :key="index" :lat-lng="mark" :icon="greyIcon" />
      <l-circle
        v-if="circle.center"
        :lat-lng="circle.center"
        :radius="circle.radius"
        :color="circle.color"
        :fillColor="circle.fillColor"
      />
    </l-map>
  </div>
</template>

<script>
import L from "leaflet";
import { LMap, LTileLayer, LMarker, LCircle } from "vue2-leaflet";
import Panel from "@/components/Panel.vue";
import LoadingMarkers from "@/components/LoadingMarkers.vue";

export default {
  name: "MyAwesomeMap",
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LCircle,
    Panel,
    LoadingMarkers,
  },
  data() {
    return {
      url: "https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}{r}.png",
      zoom: 3,
      markers: [],
      loadingMarkers: false,
      circle: {
        center: null,
        radius: 0,
        color: "#ff8080",
        fillColor: "#ff8080",
      },
    };
  },
  computed: {
    greyIcon() {
      return new L.Icon({
        iconUrl:
          "https://raw.githubusercontent.com/pointhi/leaflet-color-markers/master/img/marker-icon-2x-black.png",
        shadowUrl:
          "https://cdnjs.cloudflare.com/ajax/libs/leaflet/0.7.7/images/marker-shadow.png",
        iconSize: [25, 41],
        iconAnchor: [12, 41],
        popupAnchor: [1, -34],
        shadowSize: [41, 41],
      });
    },
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
    toggleLoadingMarkers(enable) {
      this.loadingMarkers = enable;
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
  z-index: 1;
}
</style>
