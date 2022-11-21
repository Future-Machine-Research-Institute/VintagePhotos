<template>
    <div>
        <div id="render-image"></div>
    </div>
</template>
  
<script setup lang="ts">

    import * as Three from 'three'
    import { onMounted } from 'vue'

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

    const load = (imagePath: string) => {
        loader.loadAsync(imagePath).then(texture => {
            plane.material = new Three.MeshBasicMaterial({
                map: texture
            })
            render.render(scene, camera)
        }).catch(error => {
            console.log(error)
        })
    }
    
    onMounted(() => {
        init()
        load('image.jpg')
    })

  
</script>
  
<style scoped>

#render-image {
    width: 100%;
    height: 100%;
}
 
</style>