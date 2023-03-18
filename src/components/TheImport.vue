<template>
  <div class="importer">
    <input type="file" ref="imageInput" id="imageInput" accept="image/jpeg, image/png, image/jpg" multiple @change="onFileSelected" />
    <button class="regular-button" @click="onConfirmButton">Do it!</button>
  </div>
</template>

<script setup>
import axios from 'axios';
import { ref } from 'vue';
import { v4 as uuidv4 } from 'uuid';

const emit = defineEmits(['stories-added', 'story-ready', 'closeDialogue'])

const uploadedImages = ref([])

const imageInput = ref(null)

const onFileSelected = (event) => {
  //convert file list object to an array:
  let fileList = uploadedImages.value = Object.keys(event.target.files).map(key => {
    return event.target.files[key];
  })

  uploadedImages.value = fileList.map((file) => {
    return {
      img: file,
      id: uuidv4()
    }
  })
}

const onConfirmButton = () => {

  if (uploadedImages.value.length > 0) {
    //emit stories array containing only IDs to parent comp. to form pages
    console.log("emitting stories-added")
    emit('stories-added', uploadedImages.value.map((file) => {
      return { id: file.id}
    }))

    //clear input:
    imageInput.value.value = null

    //send each image over to be encoded and sent to backend for processing + OCR
    uploadedImages.value.forEach((file) => {
      encodeImageToBase64(file.img, file.id)
    })

    //close dialogue:
    emit('closeDialogue')
  } else {
    //alert that input mustn't be empty
  }
}

const encodeImageToBase64 = (img, id) => {
  const fr = new FileReader();
  fr.onloadend = function () {
    //when done encoding, send image over to backend
    onSendImage(fr.result, id)
  }
  fr.readAsDataURL(img);
}

//Send image to backend:
const onSendImage = (base64img, id) => {
  getImageLightness(base64img).then((avgBrightness) => {
    axios.post('http://127.0.0.1:3001/upload', {
      base64img,
      avgBrightness,
      id
    }).then((res) => { //on response from server after OCR, handle text lines

      console.log(res)

      let newLines = []

      res.data.text.forEach((paragraph) => {
        paragraph.lines.forEach((line) => {
          let newLine = { ...line }
          newLine.text = line.text.replaceAll('|', 'I')
          newLines.push(newLine)
        });
      })

      emit("story-ready", {id: res.data.id, text: newLines})

    }).catch((err) => {
      console.log(err)
    })
  })
}

async function getImageLightness(imageSrc) {
  return new Promise(resolve => {
    let img = document.createElement("img");
    img.src = imageSrc;
    img.style.display = "none";
    document.body.appendChild(img);

    let colorSum = 0;

    img.onload = function () {
      // create canvas
      let canvas = document.createElement("canvas");
      canvas.width = this.width;
      canvas.height = this.height;

      let ctx = canvas.getContext("2d");
      ctx.drawImage(this, 0, 0);

      let imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
      let data = imageData.data;
      let r, g, b, avg;

      for (let x = 0, len = data.length; x < len; x += 4) {
        r = data[x];
        g = data[x + 1];
        b = data[x + 2];

        avg = Math.floor((r + g + b) / 3);
        colorSum += avg;
      }

      let brightness = Math.floor(colorSum / (this.width * this.height));
      resolve(brightness)
    }
  })
}

</script>

<style scoped>
</style>