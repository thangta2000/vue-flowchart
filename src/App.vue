<template>
  <div id="app">
    <Flowchart :chartData="data" />
    <Mermaid :chartData="data" />
  </div>
</template>

<script>
import Flowchart from "./components/flowchart"
import Mermaid from "./components/mermaid"
import data from "./data.json"

export default {
  name: "App",
  components: {
    Flowchart,
    Mermaid,
  },
  data() {
    return { data }
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
