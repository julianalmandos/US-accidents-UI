<template>
  <div class="panel">
    <div class="container">
      <h1 class="title">Panel de Control</h1>
      <CoordsSearcher v-on="$listeners" />
      <div
        :class="['subtitle-container', !graphsReady ? 'subtitle-loading-container' : 'subtitle-result-container']"
      >
        <h2 class="subtitle">Información adicional</h2>
        <img v-if="!graphsReady" class="loading-icon" src="@/assets/loading.svg" />
        <span v-else class="time-counter">
          Query time: {{audit.queryTime}}
          <span v-if="audit.cached">(cached)</span>
        </span>
      </div>

      <section v-if="graphsReady" class="information">
        <Graph v-for="graph in readyGraphs" :key="graph.name" :values="graph.value">
          <template v-slot:title>{{graph.title}}</template>
        </Graph>
      </section>
    </div>
  </div>
</template>

<script>
import Graph from "@/components/Graph.vue";
import CoordsSearcher from "@/components/CoordsSearcher.vue";
import accidentsApi from "@/gateways/accidents-api.js";
import axios from "axios";

export default {
  name: "Panel",
  components: {
    Graph,
    CoordsSearcher,
  },
  data() {
    return {
      barPixels: 3000,
      values: [],
      commonWeatherConditions: {},
      graphsReady: false,
      audit: {
        startTime: null,
        endTime: null,
        queryTime: "5.923ms",
        cached: false,
      },
      graphs: [
        {
          api: "/mostCommonConditions/weather",
          name: "Temperature",
          ready: false,
          title: "Temperatura más común (°F)",
          value: [],
        },
        {
          api: "/mostCommonConditions/location",
          name: "State",
          ready: false,
          title: "Estado con más accidentes",
          value: [],
        },
        {
          api: "/mostCommonConditions/weather",
          name: "Weather",
          ready: false,
          title: "Clima más común",
          value: [],
        },
      ],
    };
  },
  computed: {
    readyGraphs() {
      return this.graphs.filter((graph) => graph.ready);
    },
  },
  mounted() {
    if (localStorage.getItem("data")) {
      this.runQuery(this.getDataFromLocalStorage);
    } else {
      this.runQuery(this.getDataFromApi);
    }
  },
  methods: {
    async runQuery(callback) {
      this.audit.startTime = new Date();
      await callback();
      this.audit.endTime = new Date();
      this.graphsReady = true;
      this.setQueryTime();
    },
    getDataFromLocalStorage() {
      return new Promise((res, rej) => {
        const responses = localStorage.getItem("data");
        JSON.parse(responses).forEach(this.setDataFromResponse);
        this.audit.cached = true;
        res();
      });
    },
    getDataFromApi() {
      return new Promise((res, rej) => {
        axios.all(this.getApiCalls()).then(
          axios.spread((...responses) => {
            responses.forEach(this.setDataFromResponse);
            localStorage.setItem("data", JSON.stringify(responses));
            res();
          })
        );
      });
    },
    setDataFromResponse(response, index) {
      this.graphs[index].value = response.data[this.graphs[index].name];
      this.graphs[index].ready = true;
    },
    getApiCalls() {
      return this.graphs.reduce((acc, graph) => {
        return [...acc, accidentsApi.get(graph.api)];
      }, []);
    },
    setQueryTime() {
      this.audit.queryTime =
        this.audit.endTime.getTime() - this.audit.startTime.getTime() + "ms";
    },
  },
};
</script>

<style scoped>
.panel {
  background-color: rgba(255, 255, 255, 0.5);
  border-left: 15px solid var(--darkRed);
  border-radius: 20px;
  position: absolute;
  right: 25px;
  width: 23%;
  min-height: 80%;
  z-index: 2;
  display: flex;
  justify-content: center;
  -webkit-box-shadow: 0px 0px 5px 0px rgba(0, 0, 0, 0.75);
  -moz-box-shadow: 0px 0px 5px 0px rgba(0, 0, 0, 0.75);
  box-shadow: 0px 0px 5px 0px rgba(0, 0, 0, 0.75);
}

.container {
  padding: 20px 0;
  width: 90%;
}

.title {
  color: var(--darkRed);
  font-size: 1.8rem;
}

.subtitle {
  color: var(--darkRed);
  font-size: 1.4rem;
}

.loading-icon {
  width: auto;
  height: 1.4rem;
  margin-left: 10px;
}

.subtitle-loading-container {
  align-items: center;
}

.subtitle-container {
  display: flex;
  margin-top: 5px;
  white-space: nowrap;
}

.subtitle-result-container {
  align-items: baseline;
}

.time-counter {
  color: var(--grey);
  margin-left: 10px;
  font-size: 0.9rem;
  overflow: hidden;
  text-overflow: ellipsis;
}
</style>
