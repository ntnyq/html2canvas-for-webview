<script lang="ts" setup>
import { ref } from 'vue'

const messages = ref<string[]>([])

const downloadImage = (dataURL: string, filename = 'demo.png') => {
  const link = document.createElement('a')
  link.href = dataURL
  link.download = filename
  link.click()
}

const downloadScreenshot = async () => {
  addLog('Start screenshot')
  addLog('Loading dep html4canvas...')

  const html4canvas = (await import('html2canvas')).default
  const element = document.querySelector<HTMLElement>('#screenshot')!

  addLog('Start generate image')
  const canvas = await html4canvas(element, { allowTaint: true })
  const dataURL = canvas.toDataURL('image/png')

  addLog('Start download image')
  downloadImage(dataURL)
}
const addLog = (msg: string) => {
  messages.value.push(`[Info]: ${msg}`)
}
const clearLog = () => {
  messages.value = []
}
</script>

<template>
  <main id="screenshot">
    <h1>Hello world</h1>
    <div class="actions">
      <button
        @click="downloadScreenshot"
        type="button"
      >
        Download screenshot
      </button>
      <button
        @click="clearLog"
        type="button"
      >
        Clear messages
      </button>
    </div>
    <div class="messages">
      <ul>
        <li
          v-for="(message, idx) in messages"
          :key="idx"
        >
          {{ message }}
        </li>
      </ul>
    </div>
  </main>
</template>
