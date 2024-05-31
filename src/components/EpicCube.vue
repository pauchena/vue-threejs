<template>
  <div ref="threeCanvas"></div>
</template>

<script lang="ts">
import { defineComponent, onMounted, ref } from "vue";
import * as THREE from "three";

export default defineComponent({
  name: "ThreeComponent",
  setup() {
    const threeCanvas = ref<HTMLDivElement | null>(null);

    onMounted(() => {
      if (threeCanvas.value) {
        const scene = new THREE.Scene();
        const camera = new THREE.PerspectiveCamera(
          75,
          threeCanvas.value.clientWidth / threeCanvas.value.clientHeight,
          0.1,
          1000
        );
        const renderer = new THREE.WebGLRenderer();
        renderer.setSize(
          threeCanvas.value.clientWidth,
          threeCanvas.value.clientHeight
        );
        threeCanvas.value.appendChild(renderer.domElement);

        const geometry = new THREE.BoxGeometry();
        const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
        const cube = new THREE.Mesh(geometry, material);
        scene.add(cube);

        camera.position.z = 5;

        const animate = () => {
          requestAnimationFrame(animate);
          cube.rotation.x += 0.01;
          cube.rotation.y += 0.01;
          renderer.render(scene, camera);
        };

        animate();
      }
    });

    return {
      threeCanvas,
    };
  },
});
</script>

<style scoped>
div {
  width: 100%;
  height: 100vh;
}
</style>
