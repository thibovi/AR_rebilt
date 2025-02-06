<template>
  <div class="camera-container">
    <video ref="video" autoplay playsinline></video>
    <model-viewer 
      src="/models/myModel.glb" 
      alt="A 3D model" 
      ar 
      auto-rotate 
      camera-controls>
    </model-viewer>
  </div>
</template>

<script>
export default {
  name: "ArViewer",
  mounted() {
    this.startCamera();
  },
  methods: {
    async startCamera() {
      try {
        const stream = await navigator.mediaDevices.getUserMedia({ video: true });
        this.$refs.video.srcObject = stream;
      } catch (error) {
        console.error("Error accessing camera:", error);
      }
    }
  }
};
</script>

<style scoped>
.camera-container {
  position: relative;
  width: 100%;
  height: 100vh;
  overflow: hidden;
}

video {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  z-index: 0;
}

model-viewer {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 1;
}
</style>
