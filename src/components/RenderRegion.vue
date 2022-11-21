<template>
    <div>
        <div class="render-image">{{title}}</div>
    </div>
</template>
  
<script setup lang="ts">

const props = defineProps<{
  title: string
}>()
  
</script>
  
<style scoped>

.render-image {
    width: 100%;
    height: 100%;
}
 
</style>