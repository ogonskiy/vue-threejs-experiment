<template>
  <div id="container"></div>
</template>

<script>
import * as Three from "three";
const normalMapURL = require("../assets/textures/golf-ball-normal-map.png");

export default {
  name: "Scene",
  data() {
    return {
      camera: null,
      scene: null,
      renderer: null,
      mesh: null,
      pointLight: null,
      textureLoader: null,
    };
  },
  methods: {
    init: function () {
      // Loading
      this.textureLoader = new Three.TextureLoader();
      const normalTexture = this.textureLoader.load(normalMapURL);

      // Canvas wrapper
      const container = document.getElementById("container");

      // Scene
      this.scene = new Three.Scene();

      // Object
      const geometry = new Three.SphereBufferGeometry(0.5, 64, 64);

      // Material
      // const material = new Three.MeshNormalMaterial();
      const material = new Three.MeshStandardMaterial();
      material.roughness = 0.2;
      material.metalness = 0.7;
      material.color = new Three.Color("#000000");
      material.normalMap = normalTexture;

      // Mesh
      this.mesh = new Three.Mesh(geometry, material);
      this.scene.add(this.mesh);

      // Lights
      this.pointLight = new Three.PointLight(0xffffff, 1);
      this.pointLight.position.x = 2;
      this.pointLight.position.y = 3;
      this.pointLight.position.z = 4;
      this.scene.add(this.pointLight);

      this.pointLight2 = new Three.PointLight(0x0000ff, 2);
      this.pointLight2.position.set(1, 1, 1);
      this.pointLight2.intensity = 1;
      this.scene.add(this.pointLight2);

      // Camera
      this.camera = new Three.PerspectiveCamera(
        70,
        container.clientWidth / container.clientHeight,
        0.1,
        100
      );
      // this.camera = new Three.OrthographicCamera(-2, 2, 2, -2, 1, 1000);
      this.camera.position.z = 2;
      this.camera.position.x = 0;
      this.camera.position.y = 0;

      // Renderer
      this.renderer = new Three.WebGLRenderer({ antialias: true, alpha: true });
      this.renderer.setSize(container.clientWidth, container.clientHeight);

      container.appendChild(this.renderer.domElement);
    },
    animate: function () {
      // this.mesh.rotation.z += 0.01;
      // this.mesh.rotation.x += 0.01;
      this.mesh.rotation.y += 0.005;
      this.renderer.render(this.scene, this.camera);
      requestAnimationFrame(this.animate);
    },
  },
  mounted() {
    this.init();
    this.animate();
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#container {
  width: 800px;
  height: 500px;
  color: #ff5353;
  background-color: rgb(71, 71, 71);
}
</style>
