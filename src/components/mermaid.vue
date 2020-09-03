<template>
  <div class="graphDiv"></div>
</template>

<script>
/* eslint-disable */
export default {
  name: "Mermaid",
  props: {
    chartData: Object,
  },
  computed: {
    data() {
      var newArray = Object.keys(this.chartData.tasks).map(
        (key) => this.chartData.tasks[key]
      )
      return newArray
    },
  },
  methods: {
    myFunction(event, node) {
      console.log("You just clicked this node:", node)
    },
  },
  created() {
    const mermaidScript = document.createElement("script")
    mermaidScript.setAttribute(
      "src",
      "https://cdnjs.cloudflare.com/ajax/libs/mermaid/8.7.0/mermaid.min.js"
    )
    mermaidScript.async = true
    document.head.appendChild(mermaidScript)
  },
  mounted() {
    window.myFunction = this.myFunction
    setTimeout(() => {
      mermaid.mermaidAPI.initialize({
        startOnLoad: false,
      })
      var element = document.querySelector(".graphDiv")

      var insertSvg = function (svgCode, bindFunctions) {
        element.innerHTML = svgCode
      }

      var graphDefinition = "graph LR\n"

      this.data.forEach((item) => {
        if (item.nexttasks) {
          const nextTask = this.data.find(
            (i) =>
              i.id === item.nexttasks ||
              (item.nexttasks.no && i.id === item.nexttasks.no[0]) ||
              (item.nexttasks.yes && i.id === item.nexttasks.yes[0])
          )
          if (nextTask) {
            if (nextTask.type === "condition") {
              if (item.type === "start")
                graphDefinition += `${item.id}[${item.name}] --> ${nextTask.id}[${nextTask.name}]\n`
              else
                graphDefinition += `${item.id} --> ${nextTask.id}{${nextTask.name}}\n`
            } else if (item.type === "start")
              graphDefinition += `${item.id}[${item.name}] --> ${nextTask.id}[${nextTask.name}]\n`
            if (item.type === "condition") {
              const nextYesTask = this.data.find(
                (i) => i.id === item.nexttasks.yes[0]
              )
              const nextNoTask = this.data.find(
                (i) => i.id === item.nexttasks.no[0]
              )
              graphDefinition += `${item.id} --> |Yes| ${nextYesTask.id}[${nextYesTask.name}]\n${item.id}|No| --> ${nextYesTask.id}[${nextYesTask.name}]\n`
            } else
              graphDefinition += `${item.id} --> ${nextTask.id}[${nextTask.name}]\n`
          }
        }
      })
      console.log(graphDefinition)
      var graph = mermaid.mermaidAPI.render(
        "graphDiv",
        graphDefinition,
        insertSvg
      )
    }, 500)
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
</style>
