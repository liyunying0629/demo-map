<script setup type="module">
import { onMounted, onBeforeUnmount } from 'vue';
import { ref, reactive } from 'vue';
import * as THREE from "three";

const init = () => {

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
  <div class="box" ref="box">123</div>
</template>
