<template>
    <div>
        <el-button class="import-button" @click="emit('clickOnImport')">导入</el-button>
    </div>
</template>
  
<script setup lang="ts">

    const emit = defineEmits<{
        (e: 'clickOnImport'): void
    }>()
  
</script>
  
<style scoped>

.import-button {
    width: 80px;
    height: 40px;
    background-color: lightgreen;
}
  
</style>