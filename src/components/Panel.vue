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
    </div>
  </div>
</template>

<script>
export default {
  name: 'Panel',
  data() {
    return {
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
  border-left: 6px solid var(--darkRed);
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

</style>