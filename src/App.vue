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
      canvas.width = img.naturalWidth;
      canvas.height = img.naturalHeight;
      ctx.drawImage(img, 0, 0, canvas.width, canvas.height);
    };
  };

  reader.readAsDataURL(file);
}

function isSimilarColor(color1, color2) {
  const diffR = color1.r - color2.r;
  const diffG = color1.g - color2.g;
  const diffB = color1.b - color2.b;
  const diff = Math.sqrt(diffR * diffR + diffG * diffG + diffB * diffB);
  return diff < 82;
}

const onExport = () => {
  const canvas = document.getElementById('c');
  const dataURL = canvas.toDataURL('image/jpeg', 1);
  const link = document.createElement('a');
  link.href = dataURL;
  link.download = 'canvas_image.jpg';
  link.click();
}

const onChangeColor = () => {
  const canvas = document.getElementById('c');
  const ctx = canvas.getContext('2d');
  const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
  const data = imageData.data;
  const colorAInput = '#FFFFFF'
  const colorBInput = '#81CFFF'
  const colorA = {
    r: parseInt(colorAInput.slice(1, 3), 16),
    g: parseInt(colorAInput.slice(3, 5), 16),
    b: parseInt(colorAInput.slice(5, 7), 16)
  };
  const colorB = {
    r: parseInt(colorBInput.slice(1, 3), 16),
    g: parseInt(colorBInput.slice(3, 5), 16),
    b: parseInt(colorBInput.slice(5, 7), 16)
  };

  for (let i = 0; i < data.length; i += 4) {
    const r = data[i];
    const g = data[i + 1];
    const b = data[i + 2];

    if (isSimilarColor({ r, g, b }, colorA)) {
      data[i] = colorB.r
      data[i + 1] = colorB.g
      data[i + 2] = colorB.b
    }
  }

  ctx.putImageData(imageData, 0, 0);
}
</script>

<template>
  <canvas id="c"></canvas>
  <input type="file" accept="image/*" @input="onInput" />
  <button @click="onExport">导出</button>
  <button @click="onChangeColor">替换</button>
</template>

<style scoped>
canvas {
  width: 500px;
  background: rgb(116, 167, 227);
}
</style>
