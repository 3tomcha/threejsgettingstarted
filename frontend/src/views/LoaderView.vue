<template>
  <div class="loader">
    <h1>This is an loader page</h1>
  </div>
</template>

<script setup lang="ts">
import * as THREE from 'three';
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js';

// シーンを作成
const scene = new THREE.Scene();

// レンダラーを作成
const renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight);
document.body.appendChild(renderer.domElement);

// カメラを作成
const camera = new THREE.PerspectiveCamera(45, window.innerWidth / window.innerHeight, 1, 1000);
camera.position.set(0, 0, 5);

// モデルを読み込む
const loader = new GLTFLoader();
loader.load('https://ft-lab.github.io/gltf/apple/apple.glb', (gltf) => {
  // モデルを追加
  gltf.scene.traverse((object) => {
    if (object.isMesh) {
      object.castShadow = true;
      object.receiveShadow = true;
    }
  });
  gltf.scene.scale.set(10, 10, 10);
  scene.add(gltf.scene);
}, undefined, (error) => {
  console.error(error);
});

// ライトを作成
const ambientLight = new THREE.AmbientLight(0xffffff, 0.5);
scene.add(ambientLight);

const directionalLight = new THREE.DirectionalLight(0xffffff, 0.5);
directionalLight.position.set(5, 10, 7.5);
directionalLight.castShadow = true;
directionalLight.shadow.mapSize.width = 2048;
directionalLight.shadow.mapSize.height = 2048;
directionalLight.shadow.camera.near = 0.1;
directionalLight.shadow.camera.far = 50;
scene.add(directionalLight);

// レンダリングループを作成
const animate = function () {
  requestAnimationFrame(animate);

  // モデルを回転
  scene.traverse((object) => {
    if (object.isMesh) object.rotation.y += 0.01;
  });

  // レンダリング
  renderer.render(scene, camera);
};

animate();
</script>
<style>
@media (min-width: 1024px) {
  .loader {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}
</style>
