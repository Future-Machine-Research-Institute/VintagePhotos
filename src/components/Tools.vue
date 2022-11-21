<template>
    <div>
        <el-button class="import-button">导入</el-button>
    </div>
</template>
  
<script setup lang="ts">
import { method } from 'lodash';


    const emit = defineEmits<{
        (e: 'change', id: number): void
        (e: 'update', value: string): void
    }>()

    let clickOnImport = (): void => {

    }
  
</script>
  
<style scoped>

.import-button {
    width: 80px;
    height: 40px;
    background-color: lightgreen;
}
  
</style>