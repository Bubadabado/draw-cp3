<template>

<div class="draw">
  <input id="titleInput" v-model="title" placeholder="Title">
  <div id="p5sketch"></div>
  <div class="inputs">
    <button @click="download">Download</button>
    <input type="file" name="photo" @change="fileChanged">
    <button @click="upload">Upload</button>
  </div>
</div>
</template>


<script>
// @ is an alias to /src
import axios from 'axios';

export default {
  name: 'Draw',
  data() {
    return {
     title: "",
     file: null,
     p5Inst: null,
     items: [],
    }
  },
  created() {
    this.getItems();
  },
  mounted() {
    const p5 = require('p5'); 
    this.p5Inst = new p5((sk) => {
      let eraser;
      let penSlider;
      let redSlider;
      let greenSlider; 
      let blueSlider;
      let clear;
      let cnv;
      sk.setup = () => {
        cnv = sk.createCanvas(400, 400);
        cnv.addClass("p5canvas")
        eraser = sk.createCheckbox('Eraser', false);
        penSlider = sk.createSlider(10, 50, 20);
        penSlider.addClass("slider");
        redSlider = sk.createSlider(0, 255, 0);
        redSlider.addClass("slider");
        redSlider.addClass("redSlider");
        greenSlider = sk.createSlider(0, 255, 0);
        greenSlider.addClass("slider");
        greenSlider.addClass("greenSlider");
        blueSlider = sk.createSlider(0, 255, 0);
        blueSlider.addClass("slider");
        blueSlider.addClass("blueSlider");
        clear = sk.createButton('clear');
        clear.mousePressed(clearCanvas);
        sk.noStroke();
        sk.background(255);
      }
      sk.draw = () => {
        if (sk.mouseIsPressed && sk.mouseButton === sk.LEFT && sk.mouseX > 0 && sk.mouseY > 0 && sk.mouseX < sk.width && sk.mouseY < sk.height) {
          let col = sk.color(redSlider.value(), greenSlider.value(), blueSlider.value());
          sk.fill((eraser.checked()) ? 255 : col);
          sk.ellipse(sk.mouseX, sk.mouseY, penSlider.value(), penSlider.value());
        } 
      }
      let clearCanvas = () => {
        sk.background(255);
      }
      sk.saveImg = (title) => {
        sk.saveCanvas(cnv, title, 'jpg');
        //sk.save(cnv, title);
        /*sk.saveFrames(title, "jpg", 1, 1, (frames) => {
            this.file = frames[0].imageData;
        });*/
      }
    }, 'p5sketch');
  },
  methods: {
    fileChanged(event) {
      this.file = event.target.files[0]
    },
    download() {
      this.p5Inst.saveImg(this.title);
    },
    async getItems() {
      try {
        let response = await axios.get("/api/items");
        this.items = response.data;
        return true;
      } catch (error) {
        //console.log(error);
      }
    },/*
    async upload() {
      try {
        const formData = new FormData();
        //this.file = this.p5Inst.saveImg(this.title);
        this.p5Inst.saveImg(this.title);
        formData.append('photo', this.file, this.file.name)
        let r1 = await axios.post('/api/photos', formData);
        //let r2 = 
        await axios.post('/api/items', {
          title: this.title,
          //description: this.description,
          path: r1.data.path
        });
      } catch (error) {
        //console.log(error);
      }
    },*/
    async upload() {
      try {
        const formData = new FormData();
        formData.append('photo', this.file, this.file.name)
        let r1 = await axios.post('/api/photos', formData);
        let r2 = await axios.post('/api/items', {
          title: this.title,
          //description: this.description,
          path: r1.data.path
        });
        this.addItem = r2.data;
      } catch (error) {
        //console.log(error);
      }
    }
  }
}

</script>

<style scoped>
#titleInput {
  margin-left: calc(50% - 90px);
  width: 180px;
  margin-bottom: 16px;
}

.inputs {
 text-align: center;
}

/*p5js styles*/
#p5sketch{
  text-align: center;
}
</style>


<style>
/* non scoped p5js styles */
.p5canvas {
  box-shadow: 0 3px 4px rgb(105, 105, 105);
}

.slider {
  width: 80px;
  background-color: transparent;
  
  /* Removes some defaults */
  -webkit-appearance: none;
}

.slider::-webkit-slider-runnable-track {
  background: rgb(230, 230, 230);
  height: 6px;
  -webkit-appearance: none;
  
  /* Turns the cursor into the hand pointer icon when hovering over the slider  */
  cursor: pointer;
  border-radius: 4px;
}


.slider::-webkit-slider-thumb {
  width: 15px;
  height: 15px;
  background: black;
  cursor: pointer;
  -webkit-appearance: none;
  
  margin-top: -5px;

  border-radius: 8px;
  
}
.redSlider::-webkit-slider-thumb {
  background:red;
}
.greenSlider::-webkit-slider-thumb {
  background:green;
}
.blueSlider::-webkit-slider-thumb {
  background:blue;
}
</style>


