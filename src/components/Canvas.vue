<script>
import { defineComponent, ref } from "vue";
export default defineComponent({
  setup() {
    const canvas = ref(
      `<svg style="width: 100%; height: 100%" id="svg"></svg>`
    );
    const dotes = ref([]);
    const lines = ref([]);

    let pointA = [];
    let pointB = [];
    let pointC = [];
    let pointD = [];

    const makeDote = (e) => {
      let mouseX = e.pageX;
      let mouseY = e.pageY;

      let dote = `<div style=" position: absolute; left:${mouseX}px; top:${mouseY}px; width:15px; height:15px; background-color:black; border-radius:50%"></div>`;

      dotes.value.push(dote);

      if (pointA.length === 0) {
        pointA = [mouseX, mouseY];
      } else if (pointB.length === 0) {
        pointB = [mouseX, mouseY];
        drawLine(pointA, pointB);
      } else if (pointC.length === 0) {
        pointC = [mouseX, mouseY];
        drawLine(pointB, pointC);
      } else if (pointD.length === 0) {
        pointD = [mouseX, mouseY];
        drawLine(pointC, pointD);
        drawLine(pointD, pointA);
        pointA = [];
        pointB = [];
        pointC = [];
        pointD = [];
      }
      updateCanvas();
    };

    const updateCanvas = () => {
      let string = `
      ${dotes._rawValue.map((ele) => {
        return ele;
      })}
      <svg style="width: 100%; height: 100%" id="svg">
      ${lines._rawValue.map((ele) => {
        return ele;
      })}
      </svg>
      `;

      canvas.value = string.replaceAll(",", " ");
    };

    const drawLine = (pointA, pointB) => {
      console.log("this is wokring");
      let line = document.createElementNS("http://www.w3.org/2000/svg", "line");

      line.setAttribute("x1", `${pointA[0]}`);
      line.setAttribute("y1", `${pointA[1]}`);
      line.setAttribute("x2", `${pointB[0]}`);
      line.setAttribute("y2", `${pointB[1]}`);

      line.setAttribute(
        "style",
        "stroke:rgb(0, 0, 0);stroke-width:15;    position: absolute;top: 0;left: 0;"
      );

      let div = document.createElement("div");
      div.appendChild(line);
      line = div.innerHTML;
      lines.value.push(line);
    };

    return { canvas, makeDote };
  },
});
</script>
<template>
  <div
    @click="makeDote($event)"
    style="width: 100vw; height: 100vh; color: white"
    v-html="canvas"
  ></div>
</template>
