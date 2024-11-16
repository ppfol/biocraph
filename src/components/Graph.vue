<script setup>
import * as d3 from "d3";
import { ref, onMounted, watch } from 'vue';

let graph = {
  "links": [
    { "source": "18470926", "target": "10097404", "type": "CITED_BY" }
  ],
  "nodes": [
    {
      "id": "18470926"
    },
    {
      "id": "10097404"
    }
  ]
};

function go() {
  const width = window.innerWidth, height = window.innerHeight;
  const color = d3.scaleOrdinal(d3.schemeCategory10);
  const simulation = d3.forceSimulation(graph.nodes)
    .force("charge", d3.forceManyBody().strength(-3000))
    .force("center", d3.forceCenter(width / 2, height / 2))
    .force("link", d3.forceLink(graph.links).id(function (d) { return d.id; }).distance(100).strength(1))
    .on("tick", ticked);

  const svg = d3.select("#graph").append("svg")
    .attr("width", width).attr("height", height)
    .attr("style", "border: 5px solid white;")

  svg.append("defs");
  svg.select("defs").append("marker")
    .attr("id", "arrow")
    .attr("viewBox", "0 0 10 10")
    .attr("refX", 10)
    .attr("refY", 5)
    .attr("markerWidth", 10)
    .attr("markerHeight", 10)
    .attr("orient", "auto")
    .append("path")
    .attr("fill", color)
    .attr("d", 'M 0 0 L 10 5 L 0 10 z');

  const links = svg.append("g").attr("class", "links").selectAll("g")
    .data(graph.links).enter().append("g")
  links.append("line")
    .attr("stroke", color)
    .attr("stroke-width", 1)
    .attr("marker-end", `url(${new URL(`#arrow`, location)})`);
  links.append("text")
    .text(function (d) {
      return d.type;
    }).style('fill', 'white');

  const nodes = svg.append("g").attr("class", "nodes").selectAll("g")
    .data(graph.nodes).enter().append("g")
  nodes.append("circle")
    .attr("r", 5)
    .attr("fill", function (d) { return color(d.label); })
    .call(
      d3.drag()
        .on("start", dragstarted)
        .on("drag", dragged)
        .on("end", dragended)
    );
  nodes.append("text")
    .text(function (d) {
      return d.id;
    }).style('fill', 'white');

  function ticked() {
    nodes.call(updateNode);
    links.call(updateLink);
  }

  function updateLink(links) {
    links.selectAll("line")
      .attr("x1", function (d) { return d.source.x; })
      .attr("y1", function (d) { return d.source.y; })
      .attr("x2", function (d) { return d.target.x; })
      .attr("y2", function (d) { return d.target.y; });
    links.selectAll("text")
      .attr("x", function (d) {return (d.source.x+d.target.x)/2 + 5;})
      .attr("y", function (d) {return (d.source.y+d.target.y)/2 + 5;});
  }

  function updateNode(nodes) {
    nodes.selectAll("circle")
      .attr("transform", function (d) {
        return "translate(" + d.x + "," + d.y + ")";
      });
    nodes.selectAll("text")
    .attr("x", function (d) {return d.x + 5;})
    .attr("y", function (d) {return d.y + 5;});
  }

  function dragstarted(event, d) {
    event.sourceEvent.stopPropagation();
    if (!event.active) simulation.alphaTarget(0.3).restart();
    d.fx = d.x;
    d.fy = d.y;
  }

  function dragged(event, d) {
    d.fx = event.x;
    d.fy = event.y;
  }

  function dragended(event, d) {
    if (!event.active) simulation.alphaTarget(0);
    d.fx = null;
    d.fy = null;
  }

}


onMounted(() => {
  go();
});

</script>


<template>
  <div id="graph"></div>
</template>
