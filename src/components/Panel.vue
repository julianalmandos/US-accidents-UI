<template>
  <div class="panel">
    <div class="container">
      <h1 class="title">Panel de Control</h1>
      <p class="description">Seleccione el tipo de información que desea visualizar en el mapa.</p>
      <ul class="options-list">
        <li v-for="option in options" :key="option.name" class="option">
          <input class="radio" type="radio" name="option">{{option.name}}
        </li>
      </ul>
      <h2 class="subtitle">Información adicional</h2>
      <section class="information">
        <h3 class="description">Temperatura más común</h3>
        <div class="graph">
          <div class="graph-value" v-for="value in values" :key="value.name">
            <span
              class="bar"
              :style="{'width': (value.value * 100 / barPixels) + 'px'}"
            >
              {{value.value}}
            </span>
            <span class="bar-value">{{value.name}}</span>
          </div>
        </div>
        <h3 class="description">Estado con más accidentes</h3>
        <div class="graph">
          <div class="graph-value" v-for="value in values" :key="value.name">
            <span
              class="bar"
              :style="{'width': (value.value * 100 / barPixels) + 'px'}"
            >
              {{value.value}}
            </span>
            <span class="bar-value">{{value.name}}</span>
          </div>
        </div>
        <h3 class="description">Ciudad con más accidentes</h3>
        <div class="graph">
          <div class="graph-value" v-for="value in values" :key="value.name">
            <span
              class="bar"
              :style="{'width': (value.value * 100 / barPixels) + 'px'}"
            >
              {{value.value}}
            </span>
            <span class="bar-value">{{value.name}}</span>
          </div>
        </div>
        <h3 class="description">Clima más común</h3>
        <div class="graph">
          <div class="graph-value" v-for="value in values" :key="value.name">
            <span
              class="bar"
              :style="{'width': (value.value * 100 / barPixels) + 'px'}"
            >
              {{value.value}}
            </span>
            <span class="bar-value">{{value.name}}</span>
          </div>
        </div>
      </section>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Panel',
  data() {
    return {
      barPixels: 3000,
      values: [
        {
          name: '40°',
          value: '3000'
        },
        {
          name: '39°',
          value: '2500'
        },
        {
          name: '32°',
          value: '2000'
        }
      ],
      options: [
        {
          name: 'Mostrar los 5 puntos más peligrosos',
          endpoint: ''
        },
        {
          name: 'Mostrar los 5 puntos más peligrosos',
          endpoint: ''
        },
        {
          name: 'Mostrar los 5 puntos más peligrosos',
          endpoint: ''
        },
        {
          name: 'Mostrar los 5 puntos más peligrosos',
          endpoint: ''
        }
      ]
    }
  }
}
</script>

<style scoped>

.panel {
  background-color: rgba(255, 255, 255, 0.5);
  border-left: 15px solid var(--darkRed);
  border-radius: 20px;
  position: absolute;
  right: 25px;
  width: 25%;
  min-height: 80%;
  z-index: 9999;
  display: flex;
  justify-content: center;
  -webkit-box-shadow: 0px 0px 5px 0px rgba(0,0,0,0.75);
  -moz-box-shadow: 0px 0px 5px 0px rgba(0,0,0,0.75);
  box-shadow: 0px 0px 5px 0px rgba(0,0,0,0.75);
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
  margin-top: 20px;
  font-size: 1.4rem;
}

.options-list {
  list-style: none;
  margin: 20px 0 0 20px;
}

.option {
  display: block;
  margin-bottom: 8px;
}

.radio {
  margin-right: 8px;
  border: 1px solid var(--darkRed);
}

.information {
  margin-top: 20px;
}

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

.graph-value {
  height: 1.3rem;
  display: flex;
  align-items: center;
}

.graph {
  margin-top: 10px;
}

</style>