<template>
  <div class="container">
    <div>
      <img alt="Vue logo" id="img" src="@/assets/sample.jpg" />
    </div>
  </div>
</template>

<script>
import * as faceapi from 'face-api.js';

export default {
  async mounted(){
    await faceapi.nets.ssdMobilenetv1.load("/models/");
    const input = document.getElementById('img')
    const timeout = ms => {
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          reject('timeout')
        }, ms);
      });
    }
    await Promise.race([
      faceapi.detectAllFaces(input).runAndExtendWithFaceDetections(),
      timeout(5000) // 5秒以上経過で、タイムアウトさせる
    ]).then((res) => {
      if(res && res.length) {
        console.log('res',res)
        alert(`顔検出件数: ${res.length}件`)
      }
    }).catch((res) => {
      alert(`${res}: タイムアウトしました`)
    })
  }
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}
</style>
