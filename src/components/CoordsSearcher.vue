<template>
  <div>
    <p
      class="description"
    >Ingrese un par de coordenadas y un radio para mostrar los accidentes en dicha zona:</p>
    <ul class="options-list">
      <li class="option" v-for="(value, name, index) in parameters" :key="index">
        <span class="param">{{name}}:</span>
        <input class="input" type="number" name="option" v-model="parameters[name]" />
      </li>
    </ul>
    <div class="btn-container">
      <button class="btn-submit" type="submit" @click="submit">Buscar</button>
    </div>
  </div>
</template>

<script>
import axios from "@/gateways/accidents-api.js";

export default {
  name: "CoordsSearcher",
  data() {
    return {
      parameters: {
        Latitud: "",
        Longitud: "",
        Radio: "",
      },
    };
  },
  methods: {
    submit() {
      let long = this.parameters.Longitud;
      let lat = this.parameters.Latitud;
      let rad = this.parameters.Radio;
      let url = `/accidentsWithin?longitude=${long}&latitude=${lat}&radius=${rad}&limit=40`;
      axios.get(url).then((response) => {
        let coords = response.data.map((element) =>
          element.Start_Loc.coordinates.reverse()
        );
        this.$emit("update-marks", coords);
      });
    },
  },
};
</script>

<style>
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

.input {
  border: 1px solid var(--darkRed);
}

.description {
  margin-bottom: 1em;
}
</style>
