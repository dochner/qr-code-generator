<script setup>
import { ref, TransitionGroup } from 'vue'
import { saveAs } from 'file-saver'
import QRCode from 'qrcode'
import ExpandTransition from './ExpandTransition.vue';

const urlValue = ref('')
const isOpen = ref(false)

const props = defineProps({
  url: {
    type: String,
    default: 'https://qrcode.ochner.com.br'
  },

  options: {
    type: Object,
    default: () => ({})
  }
})

const qrFile = ref()

async function generateQrCode() {
  const url = props.url
  const options = {
    errorCorrectionLevel: 'H',
    type: 'image/jpeg',
    quality: 0.3,
    margin: 1,
    color: {
      dark: '#000000ff',
      light: '#ffffffff'
    },
    width: 248,
    height: 248,
    ...props.options
  }
  await QRCode.toDataURL(url, options).then(url => {
    qrFile.value = url
    isOpen.value = true
  })
}

async function downloadQrCode() {
  saveAs(qrFile.value, 'qrcode.jpg')
}

const deleteQrFile = () => {
  qrFile.value = null
  isOpen.value = false
}
</script>

<template>
  <div class="card">
    <p class="text-display">
      <code>Generate</code> your own QR code
    </p>
    <form @submit.prevent="generateQrCode" class="row text-body" >
      <div class="column">
        <label for="qrcodeContent">
            Target site (URL) of your QR code.
          <code>Enter</code> your text below:
        </label>
        <input required type="text" name="QRCode Content" placeholder="QR code value" id="qrcodeContent" :value="urlValue" @input="urlValue = $event.target.value">
      </div>
      <br>
      <div class="row mt-md">
        <button title="Delete Image" class="bg-negative" :disabled="!qrFile" @click.prevent="deleteQrFile">Delete</button>
        <button title="Download QR Code Image" class="flex-grow ml-md" type="button" @click.prevent="downloadQrCode" :disabled="!qrFile">Download</button>
        <button title="Generate QR Code Image" type="submit" class="flex-grow mt-md ml-md-md mt-md-none">Generate</button>
      </div>
    </form>

    <ExpandTransition name="slide-down">
      <div v-if="qrFile" class="qr-code items-center justify-center">
        <div class="column mt-md ">
          <img  :src="qrFile" alt="qrCode" width="248" height="248" class="mx-auto">
        </div>
      </div>
    </ExpandTransition>

    <hr class="mt-md" style="opacity: 0.4;width: 100%;">

    <div class="mt-md">
      Created by <a href="https://ochner.com.br" target="_blank" rel="noopener noreferrer">Douglas Ochner</a>
    </div>
  </div>
</template>

<style>
 .slide-down-enter-active, .slide-down-leave-active {
   transition: 300ms;

  }
 .slide-down-enter-to{
    position: relative;
    left: 0;
 }
 .slide-down-leave{
    position: absolute;
   }
 .slide-down-enter,  {
    left: -100vw;
    position: absolute;
  }
 .slide-down-leave-to {
    right: -100vw;
 }
</style>
