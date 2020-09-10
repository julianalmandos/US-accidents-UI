<template>
  <div class="home">
    <section class="welcome">
      <div class="welcome-message">
        <h1 class="title">US Accidents</h1>
        <h4 class="subtitle">
          Esta aplicación provee una interfaz gráfica para mostrar los datos procesados por la
          <a class="api-link" target="_blank" href="https://github.com/Skydler/US-accidents-API">API de US Accidents</a>.
        </h4>
        <p class="description">
          Aplicación parte del trabajo final de la asignatura Base de Datos 2, de la Facultad de Informática de la Universidad Nacional de La Plata.<br><br>
          <span class="students">Integrantes:</span> Almandos - Mandarino
        </p>
        <router-link class="map-link" to="/map"><button class="map-button"><i class="eos-icons map-icon">map</i> Ver mapa</button></router-link>
      </div>
    </section>
    <section class="image-container">
      <!--img class="flag" alt="US Flag" src="@/assets/us-flag2.png"-->
    </section>
  </div>
</template>

<script>

export default {
  name: 'Home',
}
</script>


<style scoped>

.home {
  width: 100%;
  min-height: 100vh;
  display: flex;
  align-items: center;
}

.welcome {
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  flex: 1 1 100%;
}

.image-container {
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  background-color: var(--lightRed);
  flex: 1 1 100%;
}

.flag {
  height: auto;
  width: 800px;
}

.welcome-message {
  width: 100%;
  padding: 0 120px;
}

.title {
  font-weight: 600;
  color: var(--darkRed);
}

.subtitle {
  margin-top: 20px;
  line-height: 1.2;
}

.api-link {
  text-decoration: none;
  font-weight: bold;
  color: var(--lightRed);
}

.api-link:hover {
  text-decoration: none;
  color: black;
}

.map-button {
  text-decoration: none;
  font-size: 1rem;
  margin-top: 30px;
  border: 1px solid transparent;
  background-color: var(--lightRed);
  padding: 10px 30px;
  border-radius: 10px;
  color: white;
  cursor: pointer;

  display: flex;
  align-items: center;
}

.map-button:hover {
  background-color: white;
  border: 1px solid var(--lightRed);
  color: var(--lightRed);
}

.map-link {
  text-decoration: none;
}

.map-icon {
  margin-right: 10px;
}

.students {
  font-weight: bold;
}
</style>