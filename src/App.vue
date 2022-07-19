<template>
  <div id="app">
    <!-- <router-view /> -->
    <!-- <demo-a></demo-a> -->
    <demo-b></demo-b>
  </div>
</template>
<script>
import DemoA from "./views/demoA/DemoA.vue";
import DemoB from "./views/demoB/DemoB.vue";
export default {
  components: { DemoA, DemoB },
};
</script>
<style>
</style>
