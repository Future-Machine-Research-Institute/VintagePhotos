<template>
    <div>
        <div id="render-image" class="render-image"></div>
    </div>
</template>
  
<script setup lang="ts">
    import * as Three from 'three'
    import { onMounted } from 'vue'

    // const reader: FileReader = new FileReader()

    const CopyShader = {
        uniforms: {
            'tDiffuse': { value: new Three.Texture() },
            'scaleMatrix': {value: new Three.Matrix4()}
        },

        vertexShader: /* glsl */`
                precision mediump float;
                uniform mat4 scaleMatrix;
                attribute vec2 uv;
                attribute vec3 position;
                varying vec2 vUv;
                void main() {
                    vUv = uv;
                    gl_Position = scaleMatrix * vec4( position, 1.0 );
                }`,

        fragmentShader: /* glsl */`
                precision mediump float;
                uniform sampler2D tDiffuse;
                varying vec2 vUv;
                void main() {
                    gl_FragColor = texture2D( tDiffuse, vUv );
                }`
    }

    let scene: Three.Scene
    let camera: Three.OrthographicCamera
    let render: Three.WebGLRenderer
    let plane: Three.Mesh<Three.PlaneGeometry, Three.RawShaderMaterial>

    let loader: Three.TextureLoader

    let container: HTMLElement

    const init = () => {
        container = document.getElementById('render-image')!
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
        // let planeMaterial = new Three.MeshBasicMaterial()
        let shaderMaterial = new Three.RawShaderMaterial(CopyShader)
        plane = new Three.Mesh(planeGeometry, shaderMaterial)

        scene.add(plane)

        container.appendChild(render.domElement)
        // render.render(scene, camera)
    }

    // const load = (imagePath: string) => {
    //     loader.loadAsync(imagePath).then(texture => {
    //         console.log(texture)
    //         CopyShader.uniforms.tDiffuse.value = texture
    //         console.log(plane.geometry.attributes)
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
            // console.log("pic width: ", temp.width)
            // console.log("pic height: ", temp.height)
            // console.log("container width", container.clientWidth)
            // console.log("container height", container.clientHeight)
            // console.log("projectionMatrix: ", CopyShader.uniforms.projectionMatrix)

            let scaleX = 1.0
            let scaleY = 1.0

            let widthProportion = temp.height / container.clientHeight
            let heightProportion = temp.width / container.clientWidth

            scaleX = (widthProportion >= heightProportion) ? (heightProportion / widthProportion) : 1.0
            scaleY = (widthProportion >= heightProportion) ? 1.0 : (widthProportion / heightProportion)

            CopyShader.uniforms.scaleMatrix.value.set(scaleX, 0.0, 0.0, 0.0, 0.0, scaleY, 0.0, 0.0, 0.0, 0.0, 1.0, 0.0, 0.0, 0.0, 0.0, 1.0)

            loader.loadAsync(url).then(texture => {
                windowURL.revokeObjectURL(url)
                CopyShader.uniforms.tDiffuse.value = texture
                plane.material.needsUpdate = true
                render.render(scene, camera)
            }).catch(error => {
                console.log(error)
            })
        }
    }

    const reRender = () => {
        try {
            console.log(CopyShader)
            plane.material.vertexShader = CopyShader.vertexShader
            plane.material.fragmentShader = CopyShader.fragmentShader
            plane.material.needsUpdate = true
            render.render(scene, camera)
        } catch (error) {
            alert(error)
        }
    }

    defineExpose({
        loadImage,
        reRender,
        CopyShader
    })
    
    onMounted(() => {
        init()
        window.onresize = () =>{
            console.log(container.clientWidth)
            console.log(container.clientHeight)
            let scaleX = 1.0
            let scaleY = 1.0

            let widthProportion = temp.height / container.clientHeight
            let heightProportion = temp.width / container.clientWidth

            scaleX = (widthProportion >= heightProportion) ? (heightProportion / widthProportion) : 1.0
            scaleY = (widthProportion >= heightProportion) ? 1.0 : (widthProportion / heightProportion)

            CopyShader.uniforms.scaleMatrix.value.set(scaleX, 0.0, 0.0, 0.0, 0.0, scaleY, 0.0, 0.0, 0.0, 0.0, 1.0, 0.0, 0.0, 0.0, 0.0, 1.0)
            plane.material.needsUpdate = true
            render.setSize(container.clientWidth, container.clientHeight)
            render.render(scene, camera)
        }
    })

  
</script>
  
<style scoped>

.render-image {
    width: 100%;
    height: 100%;
}
 
</style>