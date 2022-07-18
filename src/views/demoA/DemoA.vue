<template>
  <div id="three" ref="three"></div>
</template>

<script>
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
// import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader";
// import { DRACOLoader } from "three/examples/jsm/loaders/DRACOLoader";
export default {
  name: "DemoA",
  data() {
    return {
      timer: null,
    };
  },
  mounted() {
    const container = this.$refs.three;
    const renderer = new THREE.WebGLRenderer({ antialias: true }); //antialias 抗锯齿
    renderer.shadowMap.enabled = true; // 开启阴影
    renderer.setSize(window.innerWidth, window.innerHeight); // 画面宽高
    container.appendChild(renderer.domElement);
    const scene = new THREE.Scene();
    
    const camera = new THREE.PerspectiveCamera(
      100,
      window.innerWidth / window.innerHeight,
      0.1,
      1000
    );
    // const camera = new THREE.OrthographicCamera(
    //   (window.innerWidth / window.innerHeight) * 200,
    //   (window.innerWidth / window.innerHeight) * -200,
    //   200,
    //   -200,
    //   1,
    //   1000
    // );
    camera.position.set(-45, 60, 45);
    // camera.position.set(200, 300, 200);
    camera.lookAt(scene.position);

    const axesHelp = new THREE.AxesHelper(100); // 加入坐标系
    scene.add(axesHelp);

    const pointLight = new THREE.PointLight("#fff"); // 加入点光源
    pointLight.position.set(100, 200, 100);
    pointLight.castShadow = true;
    pointLight.shadow.mapSize.set(2048, 2048);
    let pointHelp = new THREE.PointLightHelper(pointLight, 50);
    scene.add(pointHelp);
    const ambienLight = new THREE.AmbientLightProbe();
    scene.add(pointLight, ambienLight);

    let plane = new THREE.PlaneGeometry(500, 300); //一个平面
    let box = new THREE.BoxGeometry(50, 50, 50); // 一个立方体

    let boxMaterial = new THREE.MeshLambertMaterial({
      color: "#333",
    });
    let planeMaterial = new THREE.MeshLambertMaterial({
      color: "#666",
      side: THREE.DoubleSide,
    });
    let planeMesh = new THREE.Mesh(plane, planeMaterial);
    let boxMesh = new THREE.Mesh(box, boxMaterial);
    scene.add(planeMesh, boxMesh);
    // planeMesh.rotation.x = Math.PI * -0.5;
    planeMesh.rotateX(Math.PI * -0.5);
    planeMesh.receiveShadow = true;
    planeMesh.position.set(0, -1, 0);
    boxMesh.position.set(0, 26, 0);
    boxMesh.castShadow = true;
    const controls = new OrbitControls(camera, container);

    let render = () => {
      renderer.render(scene, camera);
      controls.update();
      this.timer = requestAnimationFrame(render);
    };

    render();
    let distance = 2.5;
    document.onkeydown = (key) => {
      // console.log(key.key);
      switch (key.key) {
        case "a":
          boxMesh.translateX(-distance);
          break;
        case "d":
          boxMesh.translateX(distance);
          break;
        case "w":
          boxMesh.translateZ(-distance);
          break;
        case "s":
          boxMesh.translateZ(distance);
          break;
        default:
          break;
      }
    };
    // window.addEventListener();
  },
  beforeDestroy() {
    cancelAnimationFrame(this.timer);
    this.timer = null;
  },
  methods: {},
};
</script>

<style>
</style>