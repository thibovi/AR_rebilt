<template>
  <div class="camera-container">
    <video ref="video" autoplay playsinline></video>
    
    <model-viewer 
      ref="modelViewer"
      src="https://res.cloudinary.com/dzempjvto/image/upload/v1738863860/Products/1234/kgxklysjs4nl0fjcep4n.glb" 
      alt="A 3D bag model" 
      ar 
      auto-rotate 
      camera-controls>
    </model-viewer>

    <button @click="switchCamera" class="switch-button">
      📷 Wissel Camera
    </button>

    <!-- Knop om AR te starten -->
    <button @click="startAR" class="ar-button">
      🚀 Start AR
    </button>
  </div>
</template>

<script>
export default {
  name: "ArViewer",
  data() {
    return {
      currentCamera: "environment", // "environment" = achtercamera, "user" = selfiecamera
      stream: null
    };
  },
  mounted() {
    this.startCamera();
  },
  methods: {
    async startCamera() {
      if (!navigator.mediaDevices || !navigator.mediaDevices.getUserMedia) {
        console.error("Camera access is not supported in this browser or context.");
        return;
      }

      try {
        const constraints = {
          video: { facingMode: this.currentCamera }
        };

        const stream = await navigator.mediaDevices.getUserMedia(constraints);
        this.stream = stream;
        this.$refs.video.srcObject = stream;
      } catch (error) {
        console.error("Error accessing camera:", error);
      }
    },
    async switchCamera() {
      this.currentCamera = this.currentCamera === "environment" ? "user" : "environment";

      // Stop de huidige stream voordat we een nieuwe starten
      if (this.stream) {
        this.stream.getTracks().forEach(track => track.stop());
      }

      this.startCamera();
    },
    // Methode om AR te starten
    startAR() {
      const modelViewer = this.$refs.modelViewer;
      if (modelViewer && modelViewer.activateAR) {
        modelViewer.activateAR();
      } else {
        console.error("AR is not supported or the model-viewer element is not found.");
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

.switch-button {
  position: absolute;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
  background: rgba(0, 0, 0, 0.7);
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 10px;
  cursor: pointer;
  font-size: 16px;
  z-index: 2;
}
.switch-button:hover {
  background: rgba(0, 0, 0, 0.9);
}

.ar-button {
  position: absolute;
  bottom: 80px; /* Plaats de knop boven de camera-wisselknop */
  left: 50%;
  transform: translateX(-50%);
  background: rgba(0, 0, 0, 0.7);
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 10px;
  cursor: pointer;
  font-size: 16px;
  z-index: 2;
}
.ar-button:hover {
  background: rgba(0, 0, 0, 0.9);
}
</style>