<template>
  <div id="container"></div>
</template>

<script>
import * as Three from "three";

export default {
  name: "ThreeTest",
  data() {
    return {
      camera: null,
      scene: null,
      renderer: null,
      mesh: null,
      pointLight: null,
    };
  },
  methods: {
    init: function () {
      // Canvas wrapper
      const container = document.getElementById("container");

      // Scene
      this.scene = new Three.Scene();

      // Object
      let geometry = new Three.TorusGeometry(0.3, 0.2, 50, 150);

      // Material
      let material = new Three.MeshNormalMaterial();
      // material.color = new Three.Color("#ffffff");

      // Mesh
      this.mesh = new Three.Mesh(geometry, material);
      this.scene.add(this.mesh);

      // Lights
      this.pointLight = new Three.PointLight(0xffffff, 0.5);
      this.pointLight.position.x = 10;
      this.pointLight.position.y = 3;
      this.pointLight.position.z = 4;
      this.scene.add(this.pointLight);

      // Camera
      this.camera = new Three.PerspectiveCamera(
        70,
        container.clientWidth / container.clientHeight,
        0.01,
        10
      );
      // this.camera = new Three.OrthographicCamera(-2, 2, 2, -2, 1, 1000);
      this.camera.position.z = 2;

      // Renderer
      this.renderer = new Three.WebGLRenderer({ antialias: true });
      this.renderer.setSize(container.clientWidth, container.clientHeight);

      container.appendChild(this.renderer.domElement);
    },
    animate: function () {
      this.mesh.rotation.x += 0.01;
      this.mesh.rotation.y += 0.03;
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
  width: 500px;
  height: 300px;
}
</style>
