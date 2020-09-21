<template>
  <div class="loading-markers">
    <div class="loading-markers-container">
      <h3 class="loading-markers-description">Cargando marcadores...</h3>
      <small class="loading-markers-advice">(Esta operación podría tardar varios segundos)</small>
      <img class="loading-markers-icon" src="@/assets/loading.svg" />
    </div>
  </div>
</template>

<script>
export default {
  name: 'LoadingMarkers',
}
</script>

<style>
.loading-markers {
  background-color: rgba(255, 255, 255, 0.9);
  position: absolute;
  z-index: 99;
  top: 0;
  right: 0;
  left: 0;
  bottom: 0;
  display: grid;
  place-items: center;
}

.loading-markers-container {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.loading-markers-description {
  color: #999
}

.loading-markers-icon {
  margin-top: 10px;
  width: 30px;
  height: auto;
}

.loading-markers-advice {
  font-size: 0.8rem;
  color: #999;
  margin-top: 3px;
}
</style>