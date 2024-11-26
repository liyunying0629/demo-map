<script setup type="module">
import { onMounted, onBeforeUnmount } from 'vue';
import * as THREE from "three";
import { ref, reactive } from 'vue';
// 引入轨道控制器扩展库OrbitControls.js
import { OrbitControls } from 'three/addons/controls/OrbitControls.js';
//引入性能监视器stats.js
import Stats from 'three/addons/libs/stats.module.js';
// 引入dat.gui.js的一个类GUI   可视化改变三维场景
import { GUI } from 'three/addons/libs/lil-gui.module.min.js';



// 创建3D场景对象Scene
const scene = new THREE.Scene();
// //创建一个长方体几何对象Geometry
const geometry = new THREE.BoxGeometry(100, 100, 100);
//创建一个材质对象Material:MeshLambertMaterial漫反射
// const material = new THREE.MeshPhongMaterial({
//     color: 0xff0000,//0xff0000设置材质颜色为红色
//     transparent: true,//开启透明
//     opacity: 0.7,//设置透明度
// });
// 模拟镜面反射，产生一个高光效果
const material = new THREE.MeshPhongMaterial({
    color: 0xff0000,
    shininess: 20, //高光部分的亮度，默认30
    specular: 0xfffffff, //高光部分的颜色
});

// 两个参数分别为几何体geometry、材质material
const mesh = new THREE.Mesh(geometry, material); //网格模型对象Mesh
//相机在Three.js三维坐标系中的位置
// 定义相机输出画布的尺寸(单位:像素px)
// const width = window.innerWidth; //宽度
// const height = window.innerHeight; //高度
const width = 1000; //宽度
const height = 800; //高度
// 30:视场角度, width / height:Canvas画布宽高比, 1:近裁截面, 3000：远裁截面
const camera = new THREE.PerspectiveCamera(30, width / height, 1, 3000);
// 创建渲染器对象
const renderer = new THREE.WebGLRenderer({
    antialias: true,
});

// 渲染循环
const clock = new THREE.Clock();

//创建stats对象
const stats = new Stats();

// 实例化一个gui对象
const gui = new GUI();


function init() {
    // for (let i = 0; i < 10; i++) {
    //     for (let j = 0; j < 10; j++) {
    //         const mesh = new THREE.Mesh(geometry, material); //网格模型对象Mesh
    //         // 在XOZ平面上分布
    //         mesh.position.set(i * 200, 0, j * 200);
    //         scene.add(mesh); //网格模型添加到场景中
    //     }
    // }

    //设置网格模型在三维空间中的位置坐标，默认是坐标原点
    mesh.position.set(0, 0, 0);
    //把网格模型mesh添加到三维场景scene中
    scene.add(mesh);


    // 实例化一个透视投影相机对象，后面冲突所以注释
    // const camera = new THREE.PerspectiveCamera();
    // 根据需要设置相机位置具体值
    camera.position.set(400, 400, 400);
    //相机观察目标指向Threejs 3D空间中某个位置
    // camera.lookAt(0, 0, 0); //坐标原点
    camera.lookAt(200, 0, 200);//指向mesh对应的位置

    // 定义threejs输出画布的尺寸(单位:像素px)
    renderer.setSize(width, height); //设置three.js渲染区域的尺寸(像素px)
    renderer.render(scene, camera); //执行渲染操作
    // renderer.setClearColor(0x444444, 1); //设置背景颜色

    // AxesHelper：辅助观察的坐标系
    const axesHelper = new THREE.AxesHelper(150);
    scene.add(axesHelper);

    // 设置相机控件轨道控制器OrbitControls
    const controls = new OrbitControls(camera, renderer.domElement);
    // TODO controls.target.set(300, 0, 300);
    // controls.update();//update()函数内会执行camera.lookAt(controls.targe)
    // 如果OrbitControls改变了相机参数，重新调用渲染器渲染三维场景
    controls.addEventListener('change', function () {
        renderer.render(scene, camera); //执行渲染操作
    });//监听鼠标、键盘事件

    // 平行光，此外还有环境光AmbientLight、点光源PointLight
    const directionalLight = new THREE.DirectionalLight(0xffffff, 1);
    // 设置光源的方向：通过光源position属性和目标指向对象的position属性计算
    directionalLight.position.set(400, 100, 200);
    // 方向光指向对象网格模型mesh，可以不设置，默认的位置是0,0,0
    scene.add(directionalLight);
    // DirectionalLightHelper：可视化平行光
    const dirLightHelper = new THREE.DirectionalLightHelper(directionalLight, 50, 0xff0000);
    scene.add(dirLightHelper);

    document.getElementById('webgl').appendChild(renderer.domElement);
    //stats.domElement:web页面上输出计算结果,一个div元素，
    document.body.appendChild(stats.domElement);

    //改变交互界面style属性
    gui.domElement.style.right = '0px';
    gui.domElement.style.width = '300px';
    //创建一个对象，对象属性的值可以被GUI库创建的交互界面改变
    const obj = {
        x: 30,
        y: 30,
        z: 30
    };
    // gui增加交互界面，用来改变obj对应属性
    gui.add(obj, 'x', 0, 100);
    gui.add(obj, 'y', 0, 150);
    gui.add(obj, 'z', 0, 160);
    // 通过GUI改变mesh.position对象的xyz属性
    gui.add(directionalLight, 'intensity', 0, 4.0).name('平行光强度');
};

function render() {
    const spt = clock.getDelta() * 1000;//毫秒
    // console.log('两帧渲染时间间隔(毫秒)', spt);
    // console.log('帧率FPS', 1000 / spt);
    //requestAnimationFrame循环调用的函数中调用方法update(),来刷新时间
    stats.update();
    stats.setMode(0);//默认模式  0渲染帧率:刷新频率,一秒渲染次数   1渲染周期:渲染一帧多长时间(单位：毫秒ms)
    renderer.render(scene, camera); //执行渲染操作
    mesh.rotateY(0.01);//每次绕y轴旋转0.01弧度
    requestAnimationFrame(render);//请求再次执行渲染函数render，渲染下一帧
}
// onresize 事件会在窗口被调整大小时发生
window.onresize = function () {
    // 重置渲染器输出画布canvas尺寸
    renderer.setSize(window.innerWidth, window.innerHeight);
    // 全屏情况下：设置观察范围长宽比aspect为窗口宽高比
    camera.aspect = window.innerWidth / window.innerHeight;
    // 渲染器执行render方法的时候会读取相机对象的投影矩阵属性projectionMatrix
    // 但是不会每渲染一帧，就通过相机的属性计算投影矩阵(节约计算资源)
    // 如果相机的一些属性发生了变化，需要执行updateProjectionMatrix ()方法更新相机的投影矩阵
    camera.updateProjectionMatrix();
};
const destroy = () => {


};

onMounted(() => {
    init();
    render();
})
onBeforeUnmount(() => {
    destroy();
});
</script>

<template>
    <div id="webgl">123</div>
</template>
