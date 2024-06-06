<template>
  <div id="app">
    <MovieCatalog />
  </div>
</template>

<script>
//imports
import MovieCatalog from "./components/MovieCatalog.vue";

export default {
  name: "App",

  components: {
    MovieCatalog,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;

  -webkit-font-smoothing: antialiased;

  -moz-osx-font-smoothing: grayscale;

  text-align: center;

  color: #2c3e50;
}
</style>
