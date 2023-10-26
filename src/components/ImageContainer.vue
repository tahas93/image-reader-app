<template>
  <h1>Image Reader</h1>

  <div v-if="!reset" class="inner-wrapper">
    <div class="dropzone" @dragover.prevent @dragenter.prevent @dragstart.prevent
      @drop.prevent="handleFileChange($event.dataTransfer)">
      <input id="file-input" type="file" accept="image/png, image/jpeg" @change="handleFileChange($event.target)"
        required />
      <h2 v-if="!preview" for="file-input">Click or Drag N Drop Image</h2>
      <img ref="img" :src="preview" />
    </div>

    <div v-if="!load && !showResult" class="button-container">
      <button type="button" @click="read">Read</button>
    </div>

    <div v-if="load" class="progress-wrapper">
      <p class="progress" ref="status"></p>
      <p class="progress-bar"><span :style="{ width: progress + '%' }"></span></p>
    </div>
  </div>

  <div v-else class="inner-wrapper">
    <div class="result-wrapper" v-if="showResult">
      <div class="result">
        <h3>Result:</h3>
        <pre>{{ result }}</pre>
      </div>

      <div class="result">
        <h3>Words Count:</h3>
        <pre>{{ wordCount }}</pre>
      </div>

      <div class="result">
        <h3>Most Used Word:</h3>
        <ul>
          <li v-for="ele in frequency" :key="ele">{{ ele }}</li>
        </ul>
      </div>
    </div>

    <div class="button-container">
      <button type="button" @click="close">Reset</button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { createWorker, PSM, OEM } from 'tesseract.js'

const preview = ref(null)
const status = ref('')
const load = ref(false)
const img = ref(null)
const result = ref('')
const showResult = ref(false)
const progress = ref('')
const reset = ref(false)
const wordCount = ref('')
const frequency = ref([])

const handleFileChange = function (event) {
  this.file = event.files[0]

  let reader = new FileReader()
  reader.readAsDataURL(this.file)

  reader.onload = (e) => {
    preview.value = e.target.result
  }
}

const worker = createWorker({
  logger: e => {
    status.value.innerHTML = `${e.status} : ${(e.progress * 100).toFixed(2)}%`
    progress.value = (e.progress * 100).toFixed(2)
  }
})

const read = async function () {
  if (preview.value === null) {
    alert("Please insert image first.")
  } else {
    load.value = true
    await worker.load()
    await worker.loadLanguage('eng')
    await worker.initialize('eng', OEM.LSTM_ONLY)

    await worker.setParameters({
      tessedit_pageseg_mode: PSM.SINGLE_BLOCK,
    })

    const { data } = await worker.recognize(img.value)
    const listOfWordsObj = data.words
    const wordsLength = data.words.length
    console.log('data', data)
    wordCount.value = wordsLength
    result.value = data.text
    showResult.value = true
    reset.value = true
    const listOfWords = [];
    const count = {}



    for (let i = 0; i < wordsLength; i++) {
      listOfWords.push(listOfWordsObj[i].text)
    }

    for (const element of listOfWords) {
      if (count[element]) {
        count[element] += 1
      } else {
        count[element] = 1
      }
    }

    console.log('count', count)
    const keys = Object.keys(count);
    for (const ele of keys) {
      if (count[ele] > 1) {
        frequency.value.push(`${ele} : ${count[ele]}`)
      }
    }
  }
}

const close = function () {
  preview.value = ''
  status.value = ''
  load.value = false
  img.value = null
  result.value = ''
  showResult.value = false
  progress.value = ''
  reset.value = false
}
</script>
