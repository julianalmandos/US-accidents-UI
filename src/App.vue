<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<style>
:root {
  --darkRed: #f06969;
  --lightRed: #ff8080;
  --grey: #555;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  font-size: 18px;
}

h1 {
  font-size: 4rem;
}

h2 {
  font-size: 2.5rem;
}

h4 {
  font-size: 1.5rem;
}

.description {
  line-height: 1.2;
  margin-top: 10px;
  color: var(--grey);
}
</style>
