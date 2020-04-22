<template>
  <div id="app">
    <!-- <extra /> -->
    <OpenSeadragon />
  </div>
</template>

<script>
import OpenSeadragon from "@/components/OpenSeadragon";
//import extra from "@/components/extra";

export default {
  name: "app",
  components: {
    OpenSeadragon,
    //extra,
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
}

html,
body {
  width: 100%;
  height: 100%;
  overflow: hidden;
}

#app {
  height: 100%;
}
</style>
