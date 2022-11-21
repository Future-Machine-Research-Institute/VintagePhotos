<template>
  <div class="content">
    <render-region class="render-region" title = "Hello"></render-region>
    <tools class="tools"></tools>
  </div>
</template>

<script setup lang="ts">
  import RenderRegion from './RenderRegion.vue';
  import Tools from './Tools.vue';
    

</script>

<style scoped>

.content {
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    /* flex-wrap: wrap; */
}

.render-region {
  width: 100%;
  height: 80%;
  background-color: lightblue;
}

.tools {
  width: 100%;
  height: 20%;
  background-color: lightcoral;
}
</style>
