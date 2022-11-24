<template>
    <div class="tools-container">
        <el-button class="import-button" @click="importFile">导入</el-button>
        <el-input id="virtual-input" class="virtual-input" v-model="fileName" type="file" accept=".jpg,.png" @change="selectFile" />
        <el-button class="import-button" @click="emit('clickOnShader')">shader</el-button>
    </div>
</template>
  
<script setup lang="ts">
    import { ref } from 'vue';


    // const reader: FileReader = new FileReader()

    let fileName = ref('')

    const emit = defineEmits<{
        (e: 'clickOnImport', image: File): void
        (e: 'clickOnShader'): void
    }>()

    const importFile = () => {
        const input = document.getElementById('virtual-input')!
        input.click()
    }

    const selectFile = () => {
        const input = (document.getElementById('virtual-input') as HTMLInputElement)!
        const files = input.files
        if(files !== null) {
            if(files.length !== 0) {
                emit('clickOnImport', files[0])
            }
            // reader.readAsDataURL(files[0])
            // reader.onload = (e) => {
            //     if(e.target?.result instanceof ArrayBuffer) {
            //         emit('clickOnImport', e.target.result)
            //     }
            // }
        }
    }
  
</script>
  
<style scoped>

.tools-container {
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    align-items: center;
}

.import-button {
    width: 80px;
    height: 40px;
    background-color: lightgreen;
}

.virtual-input {
    width: 100%;
    height: 40px;
    position: absolute;
    top: 0px;
    left: 0px;
    visibility: hidden;
}
  
</style>