<template>
  <div>
    <div id="container" @mousemove="onMouseMove"></div>
    <div class="scene-text">THREE.JS</div>
  </div>
</template>

<script>
import * as Three from "three";
import * as dat from "dat.gui";
const normalMapURL = require("../assets/textures/golf-ball-normal-map.png");

const clock = new Three.Clock();

export default {
  name: "Scene",
  data() {
    return {
      container: null,
      camera: null,
      scene: null,
      renderer: null,
      mesh: null,
      pointLight: null,
      textureLoader: null,
      interact: {
        mouseX: 0,
        mouseY: 0,
        targetX: 0,
        targetY: 0,
      },
    };
  },
  methods: {
    init() {
      // Debug

      const gui = new dat.GUI();

      // Loading
      this.textureLoader = new Three.TextureLoader();
      const normalTexture = this.textureLoader.load(normalMapURL);

      // Canvas wrapper
      this.container = document.getElementById("container");

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
      this.pointLight2.position.set(1, 1, 0.33);
      this.pointLight2.intensity = 10;
      this.scene.add(this.pointLight2);

      const light1 = gui.addFolder("Light 1");
      light1.add(this.pointLight2.position, "x").min(-6).max(6).step(0.01);
      light1.add(this.pointLight2.position, "y").min(-3).max(3).step(0.01);
      light1.add(this.pointLight2.position, "z").min(-3).max(3).step(0.01);
      light1.add(this.pointLight2, "intensity").min(0).max(10).step(0.01);

      // const pointLightHelper = new Three.PointLightHelper(
      //   this.pointLight2,
      //   0.5
      // );
      // this.scene.add(pointLightHelper);

      this.pointLight3 = new Three.PointLight(0xff0000, 2);
      this.pointLight3.position.set(-1.05, -1.24, 0.31);
      this.pointLight3.intensity = 3.25;
      this.scene.add(this.pointLight3);

      const light2 = gui.addFolder("Light 2");
      light2.add(this.pointLight3.position, "x").min(-6).max(6).step(0.01);
      light2.add(this.pointLight3.position, "y").min(-3).max(3).step(0.01);
      light2.add(this.pointLight3.position, "z").min(-3).max(3).step(0.01);
      light2.add(this.pointLight3, "intensity").min(0).max(10).step(0.01);

      const light2Color = {
        color: "#ff0000",
      };

      light2.addColor(light2Color, "color").onChange(() => {
        this.pointLight3.color.set(light2Color.color);
      });

      // const pointLightHelper2 = new Three.PointLightHelper(
      //   this.pointLight3,
      //   0.5
      // );
      // this.scene.add(pointLightHelper2);

      // Camera
      this.camera = new Three.PerspectiveCamera(
        75,
        this.container.clientWidth / this.container.clientHeight,
        0.1,
        100
      );
      // this.camera = new Three.OrthographicCamera(-2, 2, 2, -2, 1, 1000);
      this.camera.position.z = 2;
      this.camera.position.x = 0;
      this.camera.position.y = 0;

      // Renderer
      this.renderer = new Three.WebGLRenderer({ antialias: true, alpha: true });
      this.renderer.setSize(
        this.container.clientWidth,
        this.container.clientHeight
      );

      this.container.appendChild(this.renderer.domElement);
    },
    animate() {
      // this.mesh.rotation.z += 0.01;
      // this.mesh.rotation.x += 0.01;
      this.interact.targetX = this.interact.mouseX * 0.001;
      this.interact.targetY = this.interact.mouseY * 0.001;

      this.mesh.rotation.y = 0.5 * clock.getElapsedTime();
      this.mesh.rotation.y +=
        0.5 * (this.interact.targetX - this.mesh.rotation.y);
      this.mesh.rotation.x +=
        0.5 * (this.interact.targetY - this.mesh.rotation.x);
      this.mesh.rotation.z +=
        0.5 * (this.interact.targetY - this.mesh.rotation.z);

      this.renderer.render(this.scene, this.camera);
      requestAnimationFrame(this.animate);
    },
    onMouseMove(e) {
      const sceneHalfX = window.innerWidth / 2;
      const sceneHalfY = window.innerHeight / 2;

      this.interact.mouseX = e.clientX - sceneHalfX;
      this.interact.mouseY = e.clientY - sceneHalfY;
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
  background-color: rgb(39, 39, 39);
}
.scene-text {
  font-size: 80px;
  color: white;
  font-family: sans-serif;
  text-align: center;
  height: 500px;
  line-height: 500px;
  position: absolute;
  top: 0;
  left: 0;
  width: 800px;
}
</style>
