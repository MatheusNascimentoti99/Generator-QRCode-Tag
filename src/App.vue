<template>
  <div class="container" id="page">
    <!-- <button @click="download">Download Cards</button> -->
    <div :id="'box'+value" class="box" v-for="value in qtdBoxes" :key="value">
      <canvas :id="'canvas'+value" />
      <span class="text-box">{{`${value}`.padStart(2,'0')}}</span>
    </div>
  </div>
</template>

<script>
  import HTMLT2Canvas from 'html2canvas';

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
    methods:{
      download(){
        for (let i = 1; i <= this.qtdBoxes; i++) {
          HTMLT2Canvas(document.querySelector('#box' + i))
          .then((canvas) => {
            const img = canvas.toDataURL("image/png");
            this.saveAs(img,`box${i}.png`)
          })
        }
      },
      saveAs(uri, filename) {
        var link = document.createElement('a');
        if (typeof link.download === 'string') {
          link.href = uri;
          link.download = filename;

          //Firefox requires the link to be in the body
          document.body.appendChild(link);

          //simulate click
          link.click();

          //remove the link when done
          document.body.removeChild(link);
        } else {
          window.open(uri);
        }
      }
    }
  };
</script>

<style>

  #page{
    display: flex;
  }

  .container {
    display: block;
    align-items: center;
    justify-content: space-between;
    flex-wrap: wrap;
    flex-direction: row;
  }
  .box {
    display: flex;
    width: 22cm;
    height: 7.316cm;
    justify-content: space-evenly;
    flex-direction: row;
    align-items: center;
    border: 2px solid black;
    /* margin: 0px 2px 0px 2px; */
  }

  .text-box {
    font-size: 100pt;
  }

  /* print styles */
@media print {

  body {
    margin: 0;
    color: #000;
    background-color: #dfd;
  }
}
</style>
