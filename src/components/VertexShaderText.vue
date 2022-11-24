<template>
    <div>
        <el-input class="code-input" type="textarea" v-model="vertexShaderCodeText" placeholder="Vertex Shader Code"></el-input>
        <!-- <input class="code-input" type="textarea" /> -->
    </div>
</template>

<script setup lang="ts">
    import {ref} from 'vue'

    let vertexShaderCodeText = ref('')

    defineExpose({
        vertexShaderCodeText
    })

</script>

<style scoped>

.code-input {
    width: 100%;
    height: 100%;
    background-color:beige;
}

.code-input :deep(.el-textarea__inner) {
    height: 100%;
}

</style>