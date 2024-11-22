<script setup type="module">
import { onMounted, onBeforeUnmount } from 'vue';
import * as THREE from "three";
import { ref, reactive } from 'vue';
// 引入轨道控制器扩展库OrbitControls.js
import { OrbitControls } from 'three/addons/controls/OrbitControls.js';


// document.body.appendChild(renderer.domElement);

const init = () => {
    // 创建3D场景对象Scene
    const scene = new THREE.Scene();
    //创建一个长方体几何对象Geometry
    const geometry = new THREE.BoxGeometry(100, 100, 100);
    //创建一个材质对象Material
    // const material = new THREE.MeshBasicMaterial({
    const material = new THREE.MeshLambertMaterial({
        color: 0xff0000,//0xff0000设置材质颜色为红色
        transparent: true,//开启透明
        opacity: 0.7,//设置透明度
    });
    // 两个参数分别为几何体geometry、材质material
    const mesh = new THREE.Mesh(geometry, material); //网格模型对象Mesh
    //设置网格模型在三维空间中的位置坐标，默认是坐标原点
    mesh.position.set(0, 0, 0);
    //把网格模型mesh添加到三维场景scene中
    scene.add(mesh);


    // 实例化一个透视投影相机对象，后面冲突所以注释
    // const camera = new THREE.PerspectiveCamera();
    //相机在Three.js三维坐标系中的位置
    // 定义相机输出画布的尺寸(单位:像素px)
    const width = 1000; //宽度
    const height = 700; //高度
    // 30:视场角度, width / height:Canvas画布宽高比, 1:近裁截面, 3000：远裁截面
    const camera = new THREE.PerspectiveCamera(30, width / height, 1, 3000);
    // 根据需要设置相机位置具体值
    camera.position.set(400, 400, 400);
    //相机观察目标指向Threejs 3D空间中某个位置
    // camera.lookAt(0, 0, 0); //坐标原点
    camera.lookAt(mesh.position);//指向mesh对应的位置

    // 创建渲染器对象
    const renderer = new THREE.WebGLRenderer();
    // 定义threejs输出画布的尺寸(单位:像素px)
    renderer.setSize(width, height); //设置three.js渲染区域的尺寸(像素px)
    renderer.render(scene, camera); //执行渲染操作

    // AxesHelper：辅助观察的坐标系
    const axesHelper = new THREE.AxesHelper(150);
    scene.add(axesHelper);

    // 设置相机控件轨道控制器OrbitControls
    const controls = new OrbitControls(camera, renderer.domElement);
    // 如果OrbitControls改变了相机参数，重新调用渲染器渲染三维场景
    controls.addEventListener('change', function () {
        renderer.render(scene, camera); //执行渲染操作
    });//监听鼠标、键盘事件

    //环境光:没有特定方向，整体改变场景的光照明暗
    // const ambient = new THREE.AmbientLight(0xffffff, 1);
    // scene.add(ambient);

    // 点光源
    // const pointLight = new THREE.PointLight(0xffffff, 1.0);
    // pointLight.position.set(-400, -200, -300);
    // const pointLightHelper = new THREE.PointLightHelper(pointLight, 10);
    // scene.add(pointLightHelper);

    // 平行光
    const directionalLight = new THREE.DirectionalLight(0xffffff, 1);
    // 设置光源的方向：通过光源position属性和目标指向对象的position属性计算
    directionalLight.position.set(400, 200, 100);
    // 方向光指向对象网格模型mesh，可以不设置，默认的位置是0,0,0
    scene.add(directionalLight);
    // DirectionalLightHelper：可视化平行光
    const dirLightHelper = new THREE.DirectionalLightHelper(directionalLight, 50, 0xff0000);
    scene.add(dirLightHelper);






    document.getElementById('webgl').appendChild(renderer.domElement);
};

const destroy = () => {


};

onMounted(() => {
    init();
})
onBeforeUnmount(() => {
    destroy();
});
</script>

<template>
    <div id="webgl">123</div>
</template>
