<template>
  <div class="content">
    <render-region class="render-region" ref="renderRegionView"></render-region>
    <vertex-shader-text class="vertex-shader-text" ref="vertexShaderTextView"></vertex-shader-text>
    <fragment-shader-text class="fragment-shader-text" ref="fragmentShaderTextView"></fragment-shader-text>
    <tools class="tools" @click-on-import="importImage" @click-on-shader="shaderLinked"></tools>
  </div>
</template>

<script setup lang="ts">
  import RenderRegion from './RenderRegion.vue';
  import Tools from './Tools.vue';
  import VertexShaderText from './VertexShaderText.vue';
  import FragmentShaderText from './FragmentShaderText.vue';
  import { onMounted, ref } from 'vue';

  const renderRegionView = ref()
  const vertexShaderTextView = ref()
  const fragmentShaderTextView = ref()

  const importImage = (image: File) => {
    renderRegionView.value?.loadImage(image)
  }

  const shaderLinked = () => {
    renderRegionView.value.CopyShader.vertexShader = vertexShaderTextView.value.vertexShaderCodeText
    renderRegionView.value.CopyShader.fragmentShader = fragmentShaderTextView.value.fragmentShaderCodeText
    renderRegionView.value?.reRender()
  }

  onMounted(() => {
    vertexShaderTextView.value.vertexShaderCodeText = renderRegionView.value.CopyShader.vertexShader
    fragmentShaderTextView.value.fragmentShaderCodeText = renderRegionView.value.CopyShader.fragmentShader
  })

</script>

<style scoped>
@media (min-width: 821px) {
  .content {
    width: calc(100vw - 128px);
    height: calc(100vh - 128px);
    position: relative;
    top: 64px;
    left: 64px;
    /* background-color: blue; */
  }

  .render-region {
    width: calc((100% - 60px) * 0.4);
    height: calc((100% - 60px) * 0.9);
    position: absolute;
    top: 20px;
    left: 20px;
    background-color: lightblue;
  }

  .vertex-shader-text {
    width: calc((100% - 60px) * 0.6);
    height: calc((100% - 60px) * 0.45 - 10px);
    position: absolute;
    top: 20px;
    left: calc((100% - 60px) * 0.4 + 40px);
  }

  .fragment-shader-text {
    width: calc((100% - 60px) * 0.6);
    height: calc((100% - 60px) * 0.45 - 10px);
    position: absolute;
    top: calc((100% - 60px) * 0.45 + 30px);
    left: calc((100% - 60px) * 0.4 + 40px);
  }

  .tools {
    width: calc(100% - 40px);
    height: calc((100% - 60px) * 0.1);
    position: absolute;
    top: calc((100% - 60px) * 0.9 + 40px);
    left: 20px;
    background-color: lightcoral;
  }
}

@media (max-width: 820px) {
  .content {
    width: 100vw;
    height: 100vh;
    position: relative;
    display: flex;
    /* flex-direction: column; */
    justify-content: center;
    align-items: center;
    flex-wrap: wrap;
    gap: 20px;
    /* background-color: blue; */
    overflow: auto;
  }

  .render-region {
    width: calc(100% - 40px);
    height: 50%;
    min-height: 300px;
    max-height: 600px;
    margin-top: 20px;
    background-color: lightblue;
  }

  .vertex-shader-text {
    width: calc(100% - 40px);
    height: 50%;
    min-height: 300px;
    max-height: 600px;
  }

  .fragment-shader-text {
    width: calc(100% - 40px);
    height: 50%;
    min-height: 300px;
    max-height: 600px;
  }

  .tools {
    width: calc(100% - 40px);
    height: calc((100% - 60px) * 0.1);
    margin-bottom: 20px;
    background-color: lightcoral;
  }
}
</style>
