# vue-konva-to-svg

**Extend Konva's functionality to export stages as SVG. Enhance the quality of exported images with SVG format. This is a forked project from https://github.com/dendrofen/react-konva-to-svg wrapping the functionality for Vue**

[![GitHub License](https://img.shields.io/github/license/peter-tell/vue-konva-to-svg)](LICENSE)
[![Build Size](https://img.shields.io/bundlephobia/minzip/vue-konva-to-svg?label=bundle%20size&style=flat&colorA=000000&colorB=000000)](https://bundlephobia.com/result?p=vue-konva-to-svg)
[![Version](https://img.shields.io/npm/v/vue-konva-to-svg?style=flat&colorA=000000&colorB=000000)](https://www.npmjs.com/package/vue-konva-to-svg)
[![Downloads](https://img.shields.io/npm/dt/vue-konva-to-svg.svg?style=flat&colorA=000000&colorB=000000)](https://www.npmjs.com/package/vue-konva-to-svg)

## Features

- Export Konva stages to SVG format.
- Asynchronous export with progress tracking.
- Before and after export callbacks for custom processing.
- Flexible context with a function that handles Konva stage objects.
- Export results as text SVG or Blob SVG.

## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
- [Example](#example)
- [Demo](#demo)
- [Contributing](#contributing)
- [License](#license)

## Installation

You can install `vue-konva-to-svg` using npm, yarn, or directly from GitHub.

- **npm**: `npm install vue-konva-to-svg`
- **yarn**: `yarn add vue-konva-to-svg`
- **GitHub**: [GitHub Repository](https://github.com/peter-tell/vue-konva-to-svg)

## Usage

To use `vue-konva-to-svg`, import the library and utilize the `exportStageSVG` function with your Konva stage object. This function allows you to customize the export process.

### `exportStageSVG(stage, blob, options)`

- `stage`: The Konva stage object you want to export.
- `blob` (optional): Set to `true` to export as Blob SVG, or `false` (default) to export as text SVG.
- `options` (optional): An object containing the following callbacks:
  - `onBefore`: A callback function called before export. Receives an array `[stage, layer]` as an argument.
  - `onAfter`: A callback function called after export. Receives an array `[stage, layer]` as an argument.

**Example usage:**

```javascript
import { exportStageSVG } from 'vue-konva-to-svg';

// Example usage
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
```