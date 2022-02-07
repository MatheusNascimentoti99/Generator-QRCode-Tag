<template>
  <canvas id="canvas"></canvas>
</template>

<script>
  export default {
    name: 'App',
    components: {},
    mounted() {
      var QRCode = require('qrcode');
      var canvas = document.getElementById('canvas');

      QRCode.toCanvas(canvas, 'sample text', function (error) {
        if (error) console.error(error);
        console.log('success!');
      });
    },
  };
</script>

<style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
</style>
