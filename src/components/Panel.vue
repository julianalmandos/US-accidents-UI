<template>
  <div class="panel">
    <div class="container">
      <h1 class="title">Panel de Control</h1>
      <div class="param-container">
        <p
          class="description"
        >Ingrese un par de coordenadas y un radio para mostrar los accidentes en dicha zona:</p>
        <ul class="options-list">
          <li v-for="param in parameters" :key="param.name" class="option">
            <span class="param">{{param.name}}:</span>
            <input class="input" type="number" name="option" v-model="param.value" />
          </li>
        </ul>
        <div class="btn-container">
          <button class="btn-submit" type="submit" v-on:click="submit">Buscar</button>
        </div>
      </div>

      <h2 class="subtitle">Información adicional</h2>
      <div
        :class="['subtitle-container', !graphsReady ? 'subtitle-loading-container' : 'subtitle-result-container']"
      >
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
import accidentsApi from "@/gateways/accidents-api.js";
import axios from "axios";

export default {
  name: "Panel",
  components: {
    Graph,
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
          title: "Temperatura más común",
          value: [],
        },
        // {
        //   api: '/mostCommonConditions/location',
        //   name: 'State',
        //   ready: false,
        //   title: 'Estado con más accidentes',
        //   value: []
        // },
        // {
        //   api: '/mostCommonConditions/weather',
        //   name: 'Weather',
        //   ready: false,
        //   title: 'Clima más común',
        //   value: []
        // }
      ],

      parameters: [
        {
          name: "Longitud",
          value: "",
        },
        {
          name: "Latitud",
          value: "",
        },
        {
          name: "Radio",
          value: "",
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
    this.audit.startTime = new Date();
    axios.all(this.getApiCalls()).then(
      axios.spread((...responses) => {
        responses.forEach((response, index) => {
          this.graphs[index].value = response.data[this.graphs[index].name];
          this.graphs[index].ready = true;
        });
        this.graphsReady = true;
        this.audit.endTime = new Date();
        this.setQueryTime();
      })
    );
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
  z-index: 9999;
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

.options-list {
  list-style: none;
  margin: 20px 0 0 20px;
}

.option {
  margin-bottom: 8px;
}

.input {
  border: 1px solid var(--darkRed);
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
  margin-top: 20px;
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
.param {
  width: 100px;
  display: inline-block;
  margin-right: auto;
}

.btn-submit {
  width: 70%;
  display: inline-block;
}

.btn-container {
  margin-top: 10px;
  margin-bottom: 15px;
  text-align: center;
}
</style>
