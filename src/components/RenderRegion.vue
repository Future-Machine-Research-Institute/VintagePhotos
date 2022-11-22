<template>
    <div>
        <div id="render-image"></div>
    </div>
</template>
  
<script setup lang="ts">

    import * as Three from 'three'
    import { onMounted } from 'vue'

    // const reader: FileReader = new FileReader()

    let scene: Three.Scene
    let camera: Three.OrthographicCamera
    let render: Three.WebGLRenderer
    let plane: Three.Mesh<Three.PlaneGeometry, Three.MeshBasicMaterial>

    let loader: Three.TextureLoader

    const init = () => {
        let container = document.getElementById('render-image')!
        loader = new Three.TextureLoader()
        loader.setCrossOrigin('Anonymous')
        scene = new Three.Scene()
        camera = new Three.OrthographicCamera(-1, 1, 1, -1, 0, 1)

        render = new Three.WebGLRenderer({
            antialias: true
        })
        render.setClearColor(new Three.Color(0x000000))
        render.setPixelRatio(window.devicePixelRatio)
        render.setSize(container.clientWidth, container.clientHeight)

        let planeGeometry = new Three.PlaneGeometry(2, 2)
        let planeMaterial = new Three.MeshBasicMaterial()
        plane = new Three.Mesh(planeGeometry, planeMaterial)

        scene.add(plane)

        container.appendChild(render.domElement)
        render.render(scene, camera)
    }

    // const load = (imagePath: string) => {
    //     loader.loadAsync(imagePath).then(texture => {
    //         console.log(texture)
    //         plane.material.map = texture
    //         plane.material.needsUpdate = true
    //         render.render(scene, camera)
    //     }).catch(error => {
    //         console.log(error)
    //     })
    // }

    const temp = new Image()
    const windowURL = window.URL || window.webkitURL
    const loadImage = (image: File) => {
        const url = windowURL.createObjectURL(image)
        temp.src = url
        temp.onload = () => {
            console.log(temp.width)
            console.log(temp.height)
            loader.loadAsync(url).then(texture => {
                windowURL.revokeObjectURL(url)
                plane.material.map = texture
                plane.material.needsUpdate = true
                render.render(scene, camera)
            }).catch(error => {
                console.log(error)
            })
        }
    }

    defineExpose({
        loadImage
    })
    
    onMounted(() => {
        init()
    })

  
</script>
  
<style scoped>

#render-image {
    width: 100%;
    height: 100%;
}
 
</style>