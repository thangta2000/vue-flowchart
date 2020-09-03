<template>
  <!-- <div class="graphDiv"></div> -->
  <div id="diagram"></div>
</template>

<script>
/* eslint-disable */
export default {
  name: "HelloWorld",
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
    const raphaelScript = document.createElement("script")
    const flowchartScript = document.createElement("script")
    raphaelScript.setAttribute(
      "src",
      "https://cdnjs.cloudflare.com/ajax/libs/raphael/2.3.0/raphael.min.js"
    )
    flowchartScript.setAttribute(
      "src",
      "http://flowchart.js.org/flowchart-latest.js"
    )
    raphaelScript.async = true
    flowchartScript.async = true
    document.head.appendChild(raphaelScript)
    document.head.appendChild(flowchartScript)
  },
  mounted() {
    window.myFunction = this.myFunction
    setTimeout(() => {
      const start = this.data.find((item) => item.type === "start")
      const operation = this.data.filter((item) => item.type === "regular")
      const condition = this.data.filter((item) => item.type === "condition")
      const end = this.data.find((item) => item.type === "end")

      let path = ""
      this.data.forEach((item) => {
        if (item.nexttasks) {
          const nextTask = this.data.find(
            (i) =>
              i.id === item.nexttasks ||
              (item.nexttasks.no && i.id === item.nexttasks.no[0]) ||
              (item.nexttasks.yes && i.id === item.nexttasks.yes[0])
          )
          if (nextTask) {
            if (item.type === "start") path += `st->${nextTask.id}\n`
            else if (item.type === "condition")
              path += `${item.id}(yes, right)->${item.nexttasks.yes[0]}\n${item.id}(no)->${item.nexttasks.no[0]}\n`
            else path += `${item.id}->${nextTask.id}\n`
          }
        }
      })

      const diagram = flowchart.parse(`
    ${start ? `st=>start: ${start.name}|past` : ""}
      ${end ? `e=>end: ${end.name}` : ""}
      ${
        operation
          ? operation
              .map(
                (item) => `${item.id}=>operation: ${item.name}|past:$myFunction`
              )
              .join("\n")
          : ""
      }
      ${
        condition
          ? condition
              .map((item) => `${item.id}=>condition: ${item.name}`)
              .join("\n")
          : ""
      }
      ${path}
    `)
      diagram.drawSVG("diagram", {
        // x: 30,
        // y: 50,
        "line-width": 3,
        // maxWidth: 600, //ensures the flowcharts fits within a certian width
        "line-length": 50,
        "text-margin": 10,
        "font-size": 14,
        font: "normal",
        // "font-family": "Helvetica",
        "font-weight": "normal",
        "font-color": "black",
        "line-color": "black",
        "element-color": "black",
        fill: "white",
        "yes-text": "yes",
        "no-text": "else",
        "arrow-end": "block",
        scale: 1,
        symbols: {
          start: {
            "font-color": "red",
            "element-color": "green",
            fill: "yellow",
            width: 400,
          },
          end: {
            class: "end-element",
            width: "400px",
          },
        },
        flowstate: {
          past: { fill: "#CCCCCC", maxWidth: "400" },
          current: {
            fill: "yellow",
            "font-color": "red",
            "font-weight": "bold",
          },
          future: { fill: "#FFFF99" },
          request: { fill: "blue" },
          invalid: { fill: "#444444" },
        },
      })
    }, 100)
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.end-element {
  width: 200px;
  left: -50%;
}
</style>
