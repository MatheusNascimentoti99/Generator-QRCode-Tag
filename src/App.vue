<template>
  <div class="container">
    <div class="page" :id="'page' + j" v-for="j in (0,pages)" :key="j">
      <div :id="'box'+qtdBoxes*(j-1)+i" class="box" v-for="i in (0, qtdBoxes)" :key="qtdBoxes*(j-1)+i">
        <canvas :id="`canvas${qtdBoxes*(j-1)+i}`" />
        <span class="text-box">{{`${qtdBoxes*(j-1)+i}`.padStart(2,'0')}}</span>
      </div>
    </div>
  </div>
</template>

<script>
  import HTMLT2Canvas from 'html2canvas';

  export default {
    name: 'App',
    data() {
      return {
        qtdBoxes: 100,
        pages: 4
      };
    },
    components: {},
    mounted() {
      var QRCode = require('qrcode');
      for(let j = 1; j <= this.pages; j++){
        for (let i = 1; i <= this.qtdBoxes; i++) {
          let canvas = document.getElementById(`canvas${+i+this.qtdBoxes*(j-1)}`);
          console.log(`canvas${+i+this.qtdBoxes*(j-1)}`);
          QRCode.toCanvas(canvas, `${+i+this.qtdBoxes*(j-1)}`, { scale: 7 }, function (error) {
            if (error) console.error(error);
            console.log('success!');
          });
        }
      }
      this.download();
    },
    methods:{
      download(){
        for (let i = 1; i <= this.pages; i++) {
          HTMLT2Canvas(document.querySelector('#page' + i)).then((canvas) => {
            const img = canvas.toDataURL("image/jpeg");
            this.saveAs(img,`page-retificado-${i}.jpeg`)
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

  .page{
    display: flex;
    width: 3508px !important;
    height: 4961px !important;
    margin: 16px;
    flex-direction: row;
    flex-wrap: wrap;
  }

  .container {
    align-items: center;
    justify-content: space-between;
    flex-wrap: wrap;
  }
  .box {
    display: flex;
    width: 680px;
    height: 245px;
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
