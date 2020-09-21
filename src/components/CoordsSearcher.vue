<template>
  <div>
    <p
      class="description"
    >Ingrese un par de coordenadas y un radio para mostrar los accidentes en dicha zona:</p>
    <div class="form-container">
      <ul class="options-list">
        <li class="option" v-for="(value, name, index) in parameters" :key="index">
          <span class="param">{{name}}:</span>
          <input class="input" type="number" name="option" v-model="parameters[name]" @input="setError"/>
        </li>
      </ul>
      <div class="btn-container">
        <button class="btn-submit" type="submit" @click="submit">Buscar</button>
        <small
          v-if="status.error"
          class="advice error"
        >{{status.message}}</small>
        <small
          class="advice"
        >* Las consultas están limitadas a 40 elementos por cuestiones de performance.</small>
      </div>
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
      status: {
        error: false,
        message: ''
      }
    };
  },
  methods: {
    submit() {
      this.setError();
      if (!this.validateInputs()) {
        this.setError(true, 'Por favor, ingrese los datos correspondientes.');
        return;
      }
      let long = this.parameters.Longitud;
      let lat = this.parameters.Latitud;
      let rad = this.parameters.Radio;
      let url = `/accidentsWithin?longitude=${long}&latitude=${lat}&radius=${rad}&limit=40`;
      this.$emit('loading-markers', true);
      axios.get(url).then((response) => {
        let coords = response.data.map((element) =>
          element.Start_Loc.coordinates.reverse()
        );
        let data = {
          coords: coords,
          parameters: {
            longitude: long,
            latitude: lat,
            radius: rad,
          },
        };
        this.$emit("update-marks", data);
        this.$emit('loading-markers', false);
        if (response.data.length === 0) {
          console.log('llega');
          this.setError(true, 'No se encontraron accidentes en esa zona.');
        }
      }).catch(err => {
        this.status.error = true;
        this.status.message = err.message;
        this.$emit('loading-markers', false);
      });
    },
    validateInputs() {
      return !!this.parameters.Latitud && !!this.parameters.Longitud && !!this.parameters.Radio;
    },
    setError(error = false, message = '') {
      this.status.error = error;
      this.status.message = message;
    }
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
  display: block;
}

.btn-container {
  margin-top: 10px;
  margin-bottom: 15px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.input {
  border: 1px solid var(--darkRed);
}

.description {
  margin-bottom: 1em;
}

.advice {
  margin-top: 5px;
  font-size: 0.6em;
}

.form-container {
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  align-items: center;
}

.error {
  font-size: 1rem;
  color: var(--darkRed);
}
</style>
