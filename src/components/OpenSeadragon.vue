<template>
  <div style="height: 80%">
    <!-- <span @mouseover="hover = true" @mouseleave="hover = false"> -->
    <div
      id="contentDiv"
      v-on:mousemove="updateCoordinates"
      style="float: right; width: 380px; height: 500px; margin-right:3em"
    ></div>
    <!-- <h2>{{ imageDetails }}</h2> -->
    <!-- <span v-if="hover">This is a secret message.</span> -->
    <!-- </span> -->
    <p>Coordinates: {{ x }} {{ y }}</p>
  </div>
</template>

<script>
export default {
  name: "OpenSeadragon",
  data() {
    return {
      viewer: null,
      hover: false,
      imageDetails: [],
      mousePosition: [],
      //   imagePoint: [],
      x: 0,
      y: 0,
      options: {
        id: "contentDiv",
        prefixUrl: "https://openseadragon.github.io/openseadragon/images/",
        constrainDuringPan: true,
        showNavigator: true,
      }, // openseadragon 参数配置
    };
  },
  methods: {
    pressHandler(event) {
      this.toImagePoint(event);
      this.zoomPoint(true);
    },
    updateCoordinates: function(event) {
      // pass event object, bound to mouse move with update
      this.x = event.clientX;
      this.y = event.clientY;
      this.imagePoint = event.imagePoint;
    },
    // updateImageDetails: function(event) {
    //   this.imagePoint = event.imagePoint;
    // },
    dragHandler(event) {},
    dragEndHandler(event) {},
    toImagePoint(event) {
      var webPoint = event.position;
      var viewportPoint = this.viewer.viewport.pointFromPixel(webPoint);
      var imagePoint = this.viewer.viewport.viewportToImageCoordinates(
        viewportPoint
      );
      this.imageDetails.push(viewportPoint);
      this.mousePosition.push(webPoint);
      console.log("Mouse Position: " + webPoint.toString());
      console.log("Image Coordinates:" + imagePoint.toString());
      console.log("Point From Pixel: " + viewportPoint.toString());
    },
    zoomPoint(event) {
      var zoomPoint = this.viewer.viewport.getZoom(event);
      var imageZoom = this.viewer.viewport.viewportToImageZoom(zoomPoint);
      console.log("Zoom:" + zoomPoint);
      console.log("Image Zoom: " + Math.round(imageZoom * 100) / 100);
    },
  },
  created() {},
  mounted() {
    this.viewer = OpenSeadragon(this.options);
    this.viewer.addTiledImage({
      tileSource:
        "http://openseadragon.github.io/example-images/highsmith/highsmith.dzi",
    });

    var overlay = this.viewer.paperjsOverlay();
    new OpenSeadragon.MouseTracker({
      element: this.viewer.canvas,
      pressHandler: this.pressHandler,
      dragHandler: this.dragHandler,
      dragEndHandler: this.dragEndHandler,
    }).setTracking(true);

    // new OpenSeadragon.Viewport({
    //   pointFromPixel: this.pointFromPixel,
    //   viewportToImageCoordinates: this.viewportToImageCoordinates,
    // });

    window.onresize = () => {
      overlay.resize();
      overlay.resizecanvas();
    };
  },
};
</script>

<style scoped></style>
