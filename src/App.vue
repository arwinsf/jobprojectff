<template>
  <div id="app">
    <TitleComponent></TitleComponent>
    <FetchComponent></FetchComponent>
    <GIFComponent></GIFComponent>
  </div>
</template>

<script>
import TitleComponent from "@/components/TitleComponent";
import FetchComponent from "@/components/FetchComponent";
import GIFComponent from "@/components/GIFComponent";

export default {
  name: 'App',
  components: {
    GIFComponent,
    TitleComponent,
    FetchComponent
  }
}
</script>

<style>
html {
  background-color: #2a59c0;
}

#app {
  background-color: #2a59c0;
  color: white;
  text-align: center;
  font-family: "Roboto Light", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
