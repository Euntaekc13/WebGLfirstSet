<template>
  <v-app>
    <div id="world" class="world"></div>
  </v-app>
</template>

<script>
import * as THREE from 'three'
// import { OrbitControls } from '../examples/jsm/controls/OrbitControls.js'
export default {
  name: 'Home',
  mounted() {
    const scene = new THREE.Scene()
    scene.background = new THREE.Color(0x007ca9)
    const camera = new THREE.PerspectiveCamera(45, (window.innerWidth - 20) / (window.innerHeight / 2), 0.1, 1000)
    camera.position.x = 10
    camera.position.y = 10
    camera.position.z = 10
    camera.lookAt(0, 0, 0)
    // camera.zoom = 2
    camera.fov = 50
    camera.updateProjectionMatrix()
    scene.add(camera)

    const renderer = new THREE.WebGLRenderer()
    renderer.setPixelRatio(window.devicePixelRatio)
    renderer.setSize(window.innerWidth - 20, window.innerHeight / 2)
    renderer.shadowMap.enabled = true
    const container = document.querySelector('.world')
    container.appendChild(renderer.domElement)

    const geometry = new THREE.BoxGeometry(1, 1, 1, 2, 2, 2)

    const fillMaterial = new THREE.MeshPhongMaterial({ color: 0x515151 })
    const cube = new THREE.Mesh(geometry, fillMaterial)
    const lineMaterial = new THREE.LineBasicMaterial({ color: 0xffff00 })

    const line = new THREE.LineSegments(new THREE.WireframeGeometry(geometry), lineMaterial)

    const group = new THREE.Group()
    group.add(cube)
    group.add(line)

    scene.add(group)
    render()
    function render() {
      renderer.render(scene, camera)
    }
  }
}
</script>

<style></style>
