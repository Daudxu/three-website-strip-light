
<template>
  <div class="content">
       <div class="background" ref="background"></div>
        <header>
          <div class="nav">
            <ul class="nav_list">
               <li><a>Home</a></li>
               <li><a>Three Js</a></li>
               <li><a>Background</a></li>
               <li><a>Projects</a></li>
               <li><a>Contact</a></li>
            </ul>
          </div>
        </header>
        <div class="description">
           <h1>Three JS</h1>
           <h2>How to build a difrent header <br> with three js.</h2>
        </div>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue'
import * as THREE from "three"
import bg1 from "./assets/marek-piwnicki-6vddaknKm3k-unsplash.jpg"
import bg2 from "./assets/markus-spiske-Rz8mkGw-G3w-unsplash.jpg"
import bg3 from "./assets/matthew-ball-T8L8cX4WFe4-unsplash.jpg"

let scene, camera, renderer, loader, geometry, materIal, mesh, particles, count;
const background = ref(null)
const clock = new THREE.Clock()

onMounted(() =>{
  // 场景
  scene = new THREE.Scene()
  // 相机
  camera = new THREE.PerspectiveCamera(70,window.innerWidth/window.innerHeight,0.1,1000)
  camera.position.z = 5
  // 星星几何体
  const particlesGeometry = new THREE.BufferGeometry();
  const counts = 300;
  
  const positions = new Float32Array(counts * 3);

  for(let i = 0; i < counts * 3; i++) {
      positions[i] = (Math.random() - 0.5 ) * 10;
  }

  particlesGeometry.setAttribute(
    "position",
    new THREE.BufferAttribute(positions, 3)
  )

  const particlesMaterial = new THREE.PointsMaterial();
  particlesMaterial.size = 0.01;
  particlesMaterial.sizeAttenuation = true;
  particles = new THREE.Points(particlesGeometry, particlesMaterial)
  scene.add(particles)

  // 渲染器
  const contaoner = background.value
  renderer = new THREE.WebGL1Renderer();
  renderer.setSize(window.innerWidth, window.innerHeight)
  contaoner.appendChild(renderer.domElement)
  // 材质加载器
  loader = new THREE.TextureLoader();
  // 几何体
  geometry = new THREE.PlaneGeometry(18, 8, 25, 5)
  materIal = new THREE.MeshBasicMaterial({
    map: loader.load(bg2),
    // side: THREE.DoubleSide
  })
  mesh = new THREE.Mesh(geometry, materIal);
  scene.add(mesh)
  // 获取顶点 
  count = geometry.attributes.position.count;
  
  animate()
}) 

const animate = () => {
  const time = clock.getElapsedTime()
  const elapsedTime = clock.getElapsedTime()

  particles.position.y = elapsedTime * 0.82;
  for (let i = 0; i< count; i++){
     const x = geometry.attributes.position.getX(i)
     const y = geometry.attributes.position.getY(i)

     geometry.attributes.position.setZ(i, -y * time * 0.3);
     geometry.computeVertexNormals();
     geometry.attributes.position.needsUpdate = true

  }
  requestAnimationFrame(animate)
  // mesh.rotation.z +=0.01
  renderer.render(scene, camera)
}
</script>

<style scoped>

</style>
