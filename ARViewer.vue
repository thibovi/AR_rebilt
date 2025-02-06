<template>
    <div>
      <model-viewer
        src="/path/to/your/3d-model.glb"
        ar
        ar-modes="webxr scene-viewer quick-look"
        camera-controls
        auto-rotate
        shadow-intensity="1"
        style="width: 100%; height: 500px;"
      >
      </model-viewer>
    </div>
  </template>
  
  <script>
  import '@google/model-viewer';
  
  export default {
    name: 'ARViewer',
  };
  </script>
  
  <style scoped>
    model-viewer {
        width: 100%;
        height: 500px;
    }
  </style>