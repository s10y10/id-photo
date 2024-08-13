<script setup>
const onInput = (e) => {
  const canvas = document.getElementById('c');
  const ctx = canvas.getContext('2d');

  const file = e.target.files[0];
  const reader = new FileReader();

  reader.onload = (event) => {
    const img = new Image();
    img.src = event.target.result;

    img.onload = () => {
      ctx.drawImage(img, 0, 0, canvas.width, canvas.height);
    };
  };

  reader.readAsDataURL(file);
}

const onExport = () => {
  const canvas = document.getElementById('c');
  const dataURL = canvas.toDataURL('image/jpeg');
  const link = document.createElement('a');
  link.href = dataURL;
  link.download = 'canvas_image.jpg';
  link.click();
}
</script>

<template>
  <canvas id="c" width="295" height="413"></canvas>
  <input type="file" accept="image/*" @input="onInput" />
  <button @click="onExport">导出</button>
</template>

<style scoped>
canvas {
  width: 295px;
  height: 413px;
  background: rgb(116, 167, 227);
}
</style>
