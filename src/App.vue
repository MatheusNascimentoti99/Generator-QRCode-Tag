<template>
  <div class="container">
    <div class="box" v-for="value in qtdBoxes" :key="value">
      <canvas :id="'canvas'+value" />
      <h1 class="text-box">{{`${value}`.padStart(2,'0')}}</h1>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'App',
    data() {
      return {
        qtdBoxes: 300,
      };
    },
    components: {},
    mounted() {
      var QRCode = require('qrcode');
      for (let i = 1; i <= this.qtdBoxes; i++) {
        let canvas = document.getElementById('canvas' + i);
        console.log(canvas);
        QRCode.toCanvas(canvas, i + '', { scale: 7 }, function (error) {
          if (error) console.error(error);
          console.log('success!');
        });
      }
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

  .container {
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: space-between;
    flex-wrap: wrap;
    flex-direction: row;
  }
  .box {
    display: flex;
    width: 22cm;
    height: 6cm;
    justify-content: space-evenly;
    flex-direction: row;
    align-items: center;
    border: 2px solid black;
    margin: 2px;
    padding: 0px 8px 0px 8px;
  }

  .text-box {
    font-size: 100pt;
  }
</style>
