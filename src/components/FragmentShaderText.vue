<template>
    <div>
        <el-input class="code-input" type="textarea" v-model="fragmentShaderCodeText" placeholder="Fragment Shader Code"></el-input>
    </div>
</template>

<script setup lang="ts">
    import {ref} from 'vue'

    let fragmentShaderCodeText = ref('')

    defineExpose({
        fragmentShaderCodeText
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