<script setup>
import { ref } from 'vue';
import { exportStageSVG } from '../../vue-konva-to-svg';

const stage = ref(null);
const svgOutput = ref(null);
const stageConfig = ref({
    width: 600,
    height: 400,
});
const configCircle = ref({
    x: stageConfig.value.width / 2,
    y: stageConfig.value.height / 2,
    radius: 70,
    fill: "red",
    stroke: "black",
    strokeWidth: 4,
    draggable: true
});
const configRect = ref({
    x: 20,
    y: 50,
    width: 100,
    height: 100,
    fill: 'green',
    draggable: true
});
const configText = ref({ text: 'Some text on canvas', fontSize: 24, draggable: true });

const createSVG = () => {
    exportStageSVG(stage.value.getStage(), false, {
        onBefore: ([stage, layer]) => {
            console.log('Before');
        },
        onAfter: ([stage, layer]) => {
            console.log('After');
        }
    }).then(svg => {
        svgOutput.value = '<p>The rendered SVG:</p>' + svg
    }).catch(error => {
        console.error('Error exporting SVG:', error);
    });
};

</script>
<style>
    .konvajs-content {
        border: 1px solid black;
        margin: 0 auto;
    }
</style>
<template>
    <v-stage :config="stageConfig" ref="stage">
        <v-layer>
            <v-text :config="configText"/>
            <v-rect :config="configRect"></v-rect>
            <v-circle :config="configCircle"></v-circle>
        </v-layer>
    </v-stage>
    <button @click="createSVG">Create SVG</button>
    <div v-html="svgOutput"></div>
</template>