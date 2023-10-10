<template>
  <div id="container">
    <p>Vue Konva to SVG</p>
    <KonvaStage />
  </div>
</template>
  
<script setup>
import KonvaStage from './components/KonvaStage.vue'
</script>
  
<style scoped>
#container {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
  