
<template>
  <!-- <div class="graphDiv"></div> -->
  <div id="diagram"></div>
</template>

<script>
/* eslint-disable */
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  data: function () {
    return {
      data: [
        {
          id: "1",
          text: "A",
          link: "---",
          next: ["2"],
          editable: true,
          style: "fill:#f9f,stroke:#333,stroke-width:4px",
        },
        { id: "2", text: "B", edgeType: "circle", next: ["3"] },
        { id: "3", text: "C", next: ["4", "6"] },
        { id: "4", text: "D", link: "-- This is the text ---", next: ["5"] },
        { id: "5", text: "E" },
        { id: "6", text: "F" },
      ],
    }
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
    setTimeout(() => {
      const diagram = flowchart.parse(`
      st=>start: Playbook Triggered|past
      e=>end: Done
      op1=>operation: Get File Sample From Path - D2|past:$myFunction
      op2=>operation: Get File Sample From Path - Carbon Black Enterprise Response|current
      cond=>condition: Use D2 agent
      io=>inputoutput: Use 3rd party products|request

      st->cond
      cond(yes, left)->op1->e
      cond(no, bottom)->io
      io->op2->e

      st@>op1({"stroke":"Red"})@>cond({"stroke":"Red","stroke-width":6,"arrow-end":"classic-wide-long"})@>op2({"stroke":"Red"})@>e({"stroke":"Red"})</textarea></div>
          </body>
      </html>
    `)
      diagram.drawSVG("diagram", {
        // 'x': 30,
        // 'y': 50,
        "line-width": 3,
        maxWidth: 600, //ensures the flowcharts fits within a certian width
        "line-length": 50,
        "text-margin": 10,
        "font-size": 14,
        font: "normal",
        "font-family": "Helvetica",
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
          },
          end: {
            class: "end-element",
          },
        },
        flowstate: {
          past: { fill: "#CCCCCC", "font-size": 12 },
          current: {
            fill: "yellow",
            "font-color": "red",
            "font-weight": "bold",
          },
          future: { fill: "#FFFF99" },
          request: { fill: "blue" },
          invalid: { fill: "#444444" },
          approved: {
            fill: "#58C4A3",
            "font-size": 12,
            "yes-text": "APPROVED",
            "no-text": "n/a",
          },
          rejected: {
            fill: "#C45879",
            "font-size": 12,
            "yes-text": "n/a",
            "no-text": "REJECTED",
          },
        },
      })
    }, 100)
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
svg {
  width: 100%;
}
</style>
