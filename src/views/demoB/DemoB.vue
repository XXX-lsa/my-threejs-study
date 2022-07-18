<template>
  <div>
    <div id="three" ref="three"></div>
    <div class="color-set">
      <h1>修改车身颜色</h1>
      <div class="color-container">
        <div v-for="(item, index) in colors" :key="index">
          <div
            class="color-item"
            :style="{ backgroundColor: item }"
            @click="changeColor(item)"
          ></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader";
import { DRACOLoader } from "three/examples/jsm/loaders/DRACOLoader";
export default {
  name: "DemoA",
  data() {
    return {
      timer: null,
      colors: [
        "red",
        "green",
        "blue",
        "yellow",
        "pink",
        "purple",
        "black",
        "white",
      ],
      carBody: null,
    };
  },
  mounted() {
    let scene, camera, renderer, controls, container;
    let carBody;
    let bodyMaterial = new THREE.MeshPhysicalMaterial({
      color: "#000",
      metalness: 1, // 金属度
      roughness: 0.5, // 粗糙度
      clearcoat: 1, //清漆度
      clearcoatRoughness: 0, //清漆粗糙度
    });
    container = this.$refs.three;

    // 场景
    scene = new THREE.Scene();

    // 相机
    camera = new THREE.PerspectiveCamera(
      75,
      window.innerWidth / window.innerHeight,
      0.1,
      1000
    );
    camera.position.set(0, 2, 6);
    camera.lookAt(scene.position);

    // 渲染器
    renderer = new THREE.WebGLRenderer({ antialias: true }); //antialias 抗锯齿
    renderer.setSize(window.innerWidth, window.innerHeight); // 画面宽高
    renderer.setClearColor("#000"); // 设置画面颜色
    container.appendChild(renderer.domElement);

    scene.background = new THREE.Color("#ccc"); // 设置场景背景颜色
    scene.environment = new THREE.Color("#ccc"); // 设置环境光颜色

    // 添加网格地面
    let gridHelper = new THREE.GridHelper(10, 10); // 创建一个网格帮助器，参数为网格的宽度和高度
    scene.add(gridHelper);
    gridHelper.material.opacity = 0.5; // 设置网格的透明度
    gridHelper.material.transparent = true; // 设置网格的透明度

    // 添加控制器
    controls = new OrbitControls(camera, renderer.domElement);
    // controls.autoRotate = true; // 自动旋转
    controls.enableDamping = true; // 启用阻尼
    controls.dampingFactor = 0.25; // 阻尼系数
    // controls.enableZoom = true; // 启用缩放
    // controls.minDistance = 3; // 最小距离
    // controls.maxDistance = 10; // 最大距离
    controls.minPolarAngle = 0; // 最小绕y轴角度
    // controls.maxPolarAngle = Math.PI / 2; // 最大绕y轴角度
    controls.zoomSpeed = 0.3

    // 添加坐标
    let axesHelper = new THREE.AxesHelper(10); // 创建一个坐标帮助器，参数为坐标的长度
    // scene.add(axesHelper);

    // 添加glft汽车模型
    let loader = new GLTFLoader(); // 创建一个加载器
    let dracoLoader = new DRACOLoader(); // 创建一个draco加载器
    dracoLoader.setDecoderPath("./draco/gltf/"); // 设置draco加载器的路径
    loader.setDRACOLoader(dracoLoader); // 设置加载器的draco加载器
    loader.load(
      "./model/untitled.glb", // 加载模型的路径
      // "./model/scene.gltf", // 加载模型的路径
      (gltf) => {
        let object = gltf.scene; // 获取模型的场景
        console.log(object);

        let rotate = () => {
          object.rotation.y -= 0.001;
          requestAnimationFrame(rotate);
        };
        rotate();
        object.traverse((child) => {
          if (child.isMesh) {
            console.log(child.name);
            if (child.isMesh && child.name.includes("boot001")) {
              // console.log(child.material);

              this.carBody = child;
              // child.material.color.set("#ff0000");
              // carBody.material = bodyMaterial;
              // child.material = bodyMaterial;
            }
            if (child.isMesh && child.name.indexOf("Plane_0") != -1) {
              child.visible = false; // 隐藏阴影
            }
          }
        });
        object.position.set(0, 0.7, 0); // 设置模型的位置
        scene.add(object); // 添加模型到场景
      },
      (xhr) => {
        // console.log((xhr.loaded / xhr.total) * 100 + "%");
      }
    );

    // 添加灯光
    let light1 = new THREE.DirectionalLight(0xffffff, 1); // 创建一个方向光，参数为光的颜色和强度
    light1.position.set(0, 0, 10);
    scene.add(light1);
    let light2 = new THREE.DirectionalLight(0xffffff, 1);
    light2.position.set(0, 0, -10);
    scene.add(light2);
    let light3 = new THREE.DirectionalLight(0xffffff, 1);
    light3.position.set(10, 0, 0);
    scene.add(light3);
    let light4 = new THREE.DirectionalLight(0xffffff, 1);
    light4.position.set(-10, 0, 0);
    scene.add(light4);
    let light5 = new THREE.DirectionalLight(0xffffff, 1);
    light5.position.set(0, 10, 0);
    scene.add(light5);
    let light6 = new THREE.DirectionalLight(0xffffff, 1);
    light6.position.set(0, -10, 0);
    scene.add(light6);
    let light7 = new THREE.DirectionalLight(0xffffff, 1);
    light7.position.set(10, 10, 10);
    scene.add(light7);
    let light8 = new THREE.DirectionalLight(0xffffff, 1);
    light8.position.set(10, 10, -10);
    scene.add(light8);
    let light9 = new THREE.DirectionalLight(0xffffff, 1);
    light9.position.set(-10, 10, 10);
    scene.add(light9);
    let light10 = new THREE.DirectionalLight(0xffffff, 1);
    light10.position.set(-10, 10, -10);
    scene.add(light10);

    const render = () => {
      renderer.render(scene, camera);
      controls && controls.update();
      this.timer = requestAnimationFrame(render);
    };

    render();
  },
  beforeDestroy() {
    cancelAnimationFrame(this.timer);
    this.timer = null;
  },
  methods: {
    changeColor(item) {
      this.carBody.material.color.set(item);
    },
  },
};
</script>

<style scoped>
.color-set {
  position: absolute;
  top: 0;
  left: 0;
}
.color-container {
  display: flex;
}
.color-item {
  margin: 0 5px;
  display: inline-block;
  width: 40px;
  height: 40px;
}
</style>