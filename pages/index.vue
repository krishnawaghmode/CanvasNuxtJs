<template>
    <b-container>
      <h2 class="display-5 text-center">NuxtJs Canvas Image</h2>
      <!-- class="justify-content-center" -->
      <b-row>
        <b-col lg="5">
          <img ref="bg" width="100" height="100" src="~assets/bird.jpg" style="display: none;">
          <textarea v-model="text" rows="4" cols="40" @change="drawText"></textarea>
          <br>
          Characters : {{text.length}}
          <b-form-select v-model="fontselected" :options="options" @change="handleResize"></b-form-select>
          <br>
          <!-- TextAlignX:{{textAlignX}}
          <b-form-input 
          id="range-1" 
          v-model="textAlignX" 
          type="range" 
          min="0" 
          max="1000" 
          @change="handleResize"></b-form-input>
          <br>
          TextAlignY: {{textAlignY}}
          <b-form-input 
          id="range-2" 
          v-model="textAlignY" 
          type="range" 
          min="0" 
          max="1000" 
          @change="handleResize"></b-form-input>
          <br> -->
          Canvas Width: {{width}}
          <b-form-input 
          id="range-3" 
          v-model="width" 
          type="range" 
          min="0" 
          max="1000" 
          @change="handleResize"></b-form-input>
          <br>
          Canvas Height : {{height}}
          <b-form-input 
          id="range-4" 
          v-model="height" 
          type="range" 
          min="0" 
          max="1000" 
          @change="handleResize"></b-form-input>
          <br>
          Font Size: {{fontsize}}
          <b-form-input 
          id="range-5" 
          v-model="fontsize" 
          type="range" 
          min="0" 
          max="500" 
          @change="handleResize"></b-form-input>
          <br>
          Max Width : {{maxwidth}}
          <b-form-input 
          id="range-6" 
          v-model="maxwidth" 
          type="range" 
          min="0" 
          max="1000" 
          @change="handleResize"></b-form-input>
          <br>
         <!--  Line Height : {{lineHeight}}
          <b-form-input 
          id="range-7" 
          v-model="lineHeight" 
          type="range" 
          min="0" 
          max="500" 
          @change="handleResize"></b-form-input>
          <br> -->
          <b-form-checkbox 
          id="checkbox-1" 
          v-model="textshadow" 
          name="checkbox-1" 
          value="1" 
          unchecked-value="2" 
          inline 
          @change="handleResize">Text Shadow</b-form-checkbox>
          <b-form-checkbox 
          id="checkbox-2" 
          v-model="onecolor" 
          name="checkbox-2" 
          value="1" 
          unchecked-value="2" 
          inline 
          @change="handleResize">One Color</b-form-checkbox><b-form-checkbox 
          id="checkbox-3" 
          v-model="strokeTextCheck" 
          name="checkbox-3" 
          value="1" 
          unchecked-value="2" 
          @change="handleResize" 
          inline>Stroke Text</b-form-checkbox><br><br>
          <button class="btn btn-primary" @click="handleResize">Change</button>
          <button class="btn btn-success" @click="btnDownload">Download</button>
          <button class="btn btn-danger" @click="btnDisplay">Display</button>
        </b-col>
        <b-col lg="7">
          <canvas ref="imagecanvas" width="400" height="300" style="border:2px solid #000;float: left; "></canvas>
          <img ref="imgConverted" src="" style="float: left;border:2px solid #25aabb;margin-left: 10px;">
        </b-col>
      </b-row>
    </b-container>
</template>
<script>
export default {
  data () {
    return {
      height: 400,
      width: 300,
      margin: 20,
      text: '',
      fontsize: 30,
      textshadow: '',
      strokeTextCheck: '2',
      onecolor: '',
      textAlignX: 0,
      textAlignY: 0,
      maxwidth: 300,
      lineHeight: 40,
      fontselected: null,
      options: [
        { value: null, text: 'Select Font' },
        { value: 'Hanalei', text: 'Hanalei[en]' },
        { value: 'Butcherman', text: 'Butcherman[en]' },
        { value: 'Bungee Outline', text: 'Bungee Outline[en]' },
        { value: 'Nosifer', text: 'Nosifer[en]' },
        { value: 'Akronim', text: 'Akronim[en]' },
        { value: 'Mulish', text: 'Mulish[en]' },
        { value: 'Oxygen', text: 'Oxygen[en]' },
        { value: 'Alegreya', text: 'Alegreya[en]' },
        { value: 'Roboto', text: 'Roboto[en]' },
        { value: 'Poppins', text: 'Poppins[Dev]' },
        { value: 'Kalam', text: 'Kalam[Dev]' },
        { value: 'Tillana', text: 'Tillana[Dev]' },
        { value: 'Noto Sans', text: 'Noto Sans[Dev]' },
        { value: 'Yatra One', text: 'Yatra One[Dev]' },
        { value: 'Inknut Antiqua', text: 'Inknut Antiqua[Dev]' },
        { value: 'Sura', text: 'Sura[Dev]' },
        { value: 'Mukta', text: 'Mukta[Dev]' },
        { value: 'Sumana', text: 'Sumana[Dev]' },
        { value: 'Gotu', text: 'Gotu[Dev]' },
        { value: 'Sahitya', text: 'Sahitya[Dev]' },
        { value: 'Arya', text: 'Arya[Dev]' },
        { value: 'Vesper Libre', text: 'Vesper Libre[Dev]' }
        // { value: { C: '3PO' }, text: 'This is an option with object value' },
        // { value: 'd', text: 'This one is disabled', disabled: true }
      ]
    }
  },

  computed: {
    canvas () {
      // console.log(this.$refs.imagecanvas)
      return this.$refs.imagecanvas
    },
    ctx () {
      return this.canvas.getContext('2d')
    },
    imgpreview () {
      // console.log(this.$refs.imagecanvas)
      return this.$refs.imgConverted
    },
    imgbg () {
      // console.log(this.$refs.bg)
      return this.$refs.bg
    }
  },

  mounted () {
    // window.addEventListener('resize', this.handleResize);
    this.handleResize()
  },
  methods: {
    btnDisplay () {
      const dataURI = this.canvas.toDataURL()
      // console.log(dataURI)
      this.imgpreview.src = dataURI
    },
    btnDownload () {
      // alert('Hello ')
      if (window.navigator.msSaveBlob) {
        window.navigator.msSaveBlob(this.canvas.msSaveBlob(), 'canvas-image.png')
      } else {
        const a = document.createElement('a')
        document.body.appendChild(a)
        a.href = this.canvas.toDataURL('image/png', 1.0)
        a.download = 'canvas-image.png'
        a.click()
        document.body.removeChild(a)
      }
    },
    textChange () {
      this.drawText()
    },
    handleResize () {
      // this.height = window.innerHeight - this.margin;
      // this.width = window.innerWidth - this.margin;
      this.drawText()
    },
    wrapText (ctx, text, x, y, maxWidth, lineHeight, strokeTextCheck) {
      const words = this.text.split(' ')
      let line = ''

      for (let n = 0; n < words.length; n++) {
        const testLine = line + words[n] + ' '
        const metrics = this.ctx.measureText(testLine)
        const testWidth = metrics.width
        // console.log(metrics);
        // console.log(testWidth);
        if (testWidth > maxWidth && n > 0) {
          if (strokeTextCheck === '1') {
            this.ctx.strokeText(line, x, y)
          } else {
            this.ctx.fillText(line, x, y)
          }
          line = words[n] + ' '
          y += lineHeight
        } else {
          line = testLine
        }
      }
      // this.ctx.fillText(line, x, y);
      // this.ctx.strokeText(line, x, y);

      if (strokeTextCheck === '1') {
        this.ctx.strokeText(line, x, y)
      } else {
        this.ctx.fillText(line, x, y)
      }
    },
    imgDownload () {
      // alert('Hello ')
      if (window.navigator.msSaveBlob) {
        window.navigator.msSaveBlob(this.canvas.msSaveBlob(), 'canvas-image.png')
      } else {
        const a = document.createElement('a')
        document.body.appendChild(a)
        a.href = this.canvas.toDataURL('image/png', 0.9)
        a.download = 'canvas-image.png'
        a.click()
        document.body.removeChild(a)
      }
    },
    drawText () {
      // canvas dynamic width height set
      this.canvas.width = this.width
      this.canvas.height = this.height
      // const scale = Math.min(this.canvas.width / this.imgbg.width, this.canvas.height / this.imgbg.height);
      //       const x1 = (this.canvas.width / 2) - (this.imgbg.width / 2) * scale;
      //       const y1 = (this.canvas.height / 2) - (this.imgbg.height / 2) * scale;
      //       // console.log(this.canvas.width+"  "+this.canvas.height+"   "+x1+"  "+ y1+"  "+ this.imgbg.width * scale+"  "+ this.imgbg.height * scale);
      //       this.ctx.drawImage(this.imgbg, x1, y1, this.imgbg.width * scale, this.imgbg.height * scale);
      const x = (this.canvas.width) / 2
      const y = 80
      // if ( this.textAlignY === 0) {
      //   y = (this.canvas.height) / 2;
      // } else {
      //   y = this.textAlignY;
      // }
      let grd = this.ctx.createLinearGradient(this.num1(), this.num1(), this.num1(), this.num1(), this.num1(), this.num1())

      grd.addColorStop(0.1, 'rgb(' + this.color() + ')')
      grd.addColorStop(0, 'rgb(' + this.color() + ')')
      grd.addColorStop(0.5, 'rgb(' + this.color() + ')')
      grd.addColorStop(1, 'rgb(' + this.color() + ')')

      if (this.onecolor === '1') {
        grd = 'rgb(' + this.color() + ')'
      }
      if (this.textshadow === '1') {
        this.ctx.shadowColor = 'rgb(' + this.color() + ')'
        this.ctx.shadowOffsetX = 0
        this.ctx.shadowOffsetY = 0
        this.ctx.shadowBlur = 10
      }
      this.ctx.restore()
      this.ctx.font = 'bold ' + this.fontsize + 'px ' + this.fontselected
      this.ctx.clearRect(0, 0, this.canvas.width, this.canvas.height)
      if (this.strokeTextCheck === '1') {
        this.ctx.strokeStyle = grd
      } else {
        this.ctx.fillStyle = grd
      }
      this.ctx.textAlign = 'center'
      this.wrapText(this.ctx, this.text, x, y, this.maxwidth, this.lineHeight, this.strokeTextCheck)
    },
    color () {
      const r = Math.floor(Math.random() * 255) + 1
      const g = Math.floor(Math.random() * 255) + 1
      const b = Math.floor(Math.random() * 255) + 1

      const color = r + ',' + g + ',' + b
      return color
    },
    num1 () {
      return Math.floor(Math.random() * 255) + 1
    }
  }
}
</script>
<style>
@import url('https://fonts.googleapis.com/css2?family=Mulish:wght@900&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Kalam:wght@300&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Tillana&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Noto+Sans&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Yatra+One&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Inknut+Antiqua:wght@800&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Sura:wght@700&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Hanalei&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Butcherman&family=Hanalei&display=swap');
/*Bungee Outline*/
@import url('https://fonts.googleapis.com/css2?family=Bungee+Outline&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Beth+Ellen&display=swap');
/*drop water*/
@import url('https://fonts.googleapis.com/css2?family=Nosifer&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Akronim&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Mulish&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Oxygen&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Poppins&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Alegreya&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Roboto&display=swap');

/*Devnagari font */
@import url('https://fonts.googleapis.com/css2?family=Kalam:wght@700&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Tillana:wght@500&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Noto+Sans:ital,wght@1,700&family=Yatra+One&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Yatra+One&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Inknut+Antiqua:wght@800&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Sura:wght@700&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Mukta:wght@800&family=Sura:wght@700&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Sumana:wght@700&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Gotu&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Sahitya:wght@700&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Arya:wght@700&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Vesper+Libre:wght@900&display=swap');

</style>
