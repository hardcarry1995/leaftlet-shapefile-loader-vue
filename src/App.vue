<template>
  <div id="app">
    <MapViewer />
  </div>
</template>

<script>
import MapViewer from './components/MapViewer/index.vue'

export default {
  components: {
    MapViewer
  }
}
</script>

<style>
body {
  margin: 0;
  height: 100vh;
}

#app {
  height: 100%;
}
</style>
