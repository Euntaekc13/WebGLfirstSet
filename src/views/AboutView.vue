<template>
  <div class="container"></div>
</template>

<script>
import * as THREE from 'three'
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls'
export default {
  name: 'About',
  mounted() {
    const scene = new THREE.Scene()
    // console.log(scene)
    scene.background = new THREE.Color(0x007ca9)
    const camera = new THREE.PerspectiveCamera(45, (window.innerWidth - 20) / (window.innerHeight / 2), 0.1, 1000)

    camera.position.x = 10
    camera.position.y = 10
    camera.position.z = 10

    camera.lookAt(0, 0, 0)
    // camera.zoom = 2
    camera.fov = 45
    camera.updateProjectionMatrix()
    scene.add(camera)

    const renderer = new THREE.WebGLRenderer()
    renderer.setPixelRatio(window.devicePixelRatio)
    renderer.setSize(window.innerWidth - 20, window.innerHeight / 2)
    renderer.shadowMap.enabled = true
    const container = document.querySelector('.container')
    container.appendChild(renderer.domElement)

    // const ambientLight = new THREE.AmbientLight(0xff0000, 1)
    // const directionalLight = new THREE.DirectionalLight(0xffffff, 1)
    // directionalLight.position.x += 2
    // directionalLight.position.z += 2
    // scene.add(directionalLight)
    // const hemisphereLight = new THREE.HemisphereLight(0x0000ff, 0xff0000, 1)
    // scene.add(hemisphereLight)

    //포인트 조명
    // const pointLight = new THREE.PointLight(0x10ff05, 1, 500)
    // pointLight.position.set(50, 50, 50)
    // scene.add(pointLight)

    //스포트라이트 조명
    const light = new THREE.SpotLight(0xfffff0, 1, 50, 0.45, 1, 2)
    light.position.y = 10
    scene.add(light)

    //바닥 판
    const plane = new THREE.PlaneGeometry(10, 10)
    const planemeterial = new THREE.MeshStandardMaterial()
    const planemesh = new THREE.Mesh(plane, planemeterial)
    planemesh.rotateX(-(Math.PI / 2)) // ????????
    planemesh.position.set(0, 0, 0)
    scene.add(planemesh)
    scene.add(new THREE.AxesHelper(5))

    //물체 (사각)
    const geometry = new THREE.BoxGeometry(2, 2, 2)
    // const meterial = new THREE.MeshBasicMaterial({ color: 0x00ff00 })
    const meterial = new THREE.MeshStandardMaterial()
    const mesh = new THREE.Mesh(geometry, meterial.clone())
    // meterial.wireframe = true //와이어틀
    meterial.metalness = true // 금속재질
    // meterial.transparent = true //투명도 사용
    // meterial.opacity = 0.8 //투명 정도
    // meterial.roughness = 0.3 //표면 거친정도
    mesh.position.set(-2, 3, 1)
    scene.add(mesh) //mesh => 객체

    mesh.add(new THREE.AxesHelper(5))

    //물체(실린더)
    const geometrycircle = new THREE.CylinderGeometry(1, 0, 1, 3)
    // const circlemeterial = new THREE.MeshStandardMaterial()
    const circlemesh = new THREE.Mesh(geometrycircle, meterial.clone())
    circlemesh.position.set(2, 3, -1)
    scene.add(circlemesh)
    circlemesh.add(new THREE.AxesHelper(3))

    //그리드 좌표계
    const grid = new THREE.GridHelper(30, 10)
    scene.add(grid)

    //x,y,z좌표계

    //마우스 컨트롤
    const control = new OrbitControls(camera, renderer.domElement)
    control.update()
    const raycaster = new THREE.Raycaster()
    const pointer = new THREE.Vector2()

    //마우스 클릭 좌표값
    renderer.domElement.addEventListener('pointerdown', event => {
      // console.log(event.clientX - event.offsetX)
      const cx = event.clientX //div + 빈공간 x좌표
      const tx = event.offsetX //div x좌표
      const cy = event.clientY //div + 빈공간 y좌표
      const ty = event.offsetY
      const boundx = cx - tx
      const boundy = cy - ty
      const rx = cx - boundx
      const ry = cy - boundy
      console.log('x:', rx, 'y:', ry)

      const width = renderer.domElement.clientWidth
      const height = renderer.domElement.clientHeight

      console.log(width, height)
      const x1 = rx / width
      const y1 = ry / height
      // console.log(x1, y1)

      const wX = x1 * 2 - 1
      const wY = -y1 * 2 + 1
      console.log(wX, wY)

      //카메라 죄표값
      pointer.set(wX, wY)
      raycaster.setFromCamera(pointer, camera)
      //object 선택
      const intersects = raycaster.intersectObjects(scene.children)
      //선택한 object 색변화
      if (intersects.length > 0) {
        const intersect = intersects[0]
        intersect.object.material.color.set(THREE.MathUtils.randInt(0x000000, 0xffffff))
        console.log(intersect)
      }
    })

    render()

    function render() {
      renderer.render(scene, camera)

      mesh.rotation.x += 0.06
      mesh.rotation.y += 0.06

      circlemesh.rotation.x += 0.01
      circlemesh.rotation.y += 0.01

      requestAnimationFrame(render)
    }
  }
}
</script>

<style lang="scss" scoped></style>
