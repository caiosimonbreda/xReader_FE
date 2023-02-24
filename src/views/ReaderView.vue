<template>
  <Navbar class="navbar" />
  <input type="file" id="imageInput" accept="image/jpeg, image/png, image/jpg" @change="onFileSelected" />
  <button class="regular-button" @click="onConfirmButton">Read</button>

  <div class="text-wrapper">
    <p v-for="line in textLines">{{ line.text }}</p>
  </div>
</template>

<script setup>
import axios from 'axios';
import { ref } from 'vue';
import Navbar from '../components/TheNavbar.vue'

const uploadedImage = ref("")
const textLines = ref("")

const onFileSelected = (event) => {
  uploadedImage.value = event.target.files[0]
}

const onConfirmButton = () => {
  if (uploadedImage.value !== "") {
    encodeImageToBase64(uploadedImage.value)
  }
}

//Send image to backend:
const onSendImage = (base64img) => {
  getImageLightness(base64img).then((avgBrightness) => {
    axios.post('http://127.0.0.1:3001/upload', {
      base64img,
      avgBrightness
    }).then((res) => { //on response from server after OCR, handle text lines

      console.log(res)

      let newLines = []

      res.data.forEach((paragraph) => {
        paragraph.lines.forEach((line) => {
          let newLine = {...line}
          newLine.text = line.text.replaceAll('|', 'I')
          newLines.push(newLine)
        });
      })

      console.log(newLines)

      textLines.value = newLines
    }).catch((err) => {
      console.log(err)
    })
  })
}

const encodeImageToBase64 = (img) => {
  const fr = new FileReader();
  fr.onloadend = function () {
    //when done encoding, send image over to backend
    onSendImage(fr.result)
  }
  fr.readAsDataURL(img);
}

async function getImageLightness(imageSrc) {
  return new Promise(resolve => {
    var img = document.createElement("img");
    img.src = imageSrc;
    img.style.display = "none";
    document.body.appendChild(img);

    var colorSum = 0;

    img.onload = function () {
      // create canvas
      var canvas = document.createElement("canvas");
      canvas.width = this.width;
      canvas.height = this.height;

      var ctx = canvas.getContext("2d");
      ctx.drawImage(this, 0, 0);

      var imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
      var data = imageData.data;
      var r, g, b, avg;

      for (var x = 0, len = data.length; x < len; x += 4) {
        r = data[x];
        g = data[x + 1];
        b = data[x + 2];

        avg = Math.floor((r + g + b) / 3);
        colorSum += avg;
      }

      var brightness = Math.floor(colorSum / (this.width * this.height));
      resolve(brightness)
    }
  })
}
</script>

<style scoped>
.navbar {
  position: absolute;
  width: 100vw;
  bottom: 0;
  max-width: 100%;
}

input[type="file"] {
  display: block;
}

input {
  color: white;
  border: 1px solid #555;
  border-radius: 10px;
  padding: 1em;
  width: 26em;
  margin: auto;
}
.regular-button {
  padding: 1em;
  border: none;
  color: white;
  border-radius: 7px;
  background-color: #555;
  margin: auto;
}
.regular-button:hover,
.regular-button:active {
  background-color: #888;
}
.text-wrapper {
  margin: auto;
  width: 90%;
  font-family: 'Source Code Pro', 'Courier New', Courier, monospace;
  color: #94bc88
}
</style>


