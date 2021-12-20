<template>
  <div v-show="isShow" class="fixed right-0 bottom-0 m-5 z-100">
    <div class="flex justify-between items-center ">
      <div class="text-xl cursor-pointer" @click="() => isShow = !isShow">
        <pixelarticons-close />
      </div>
      <button class="btn cursor-pointer">
        <a :href="videoPath" download>Download video</a>
      </button>
    </div>

    <video ref="player" class="w-[300px]" controls>
    </video>
  </div>
</template>

<script setup lang="ts">
import { useMagicKeys } from '@vueuse/core'
const { ctrl, alt, r } = useMagicKeys()
const player = ref<HTMLVideoElement | null>(null)
const isShow = ref<Boolean>(false)
const videoPath = ref<string>('')
const startShareScreen = async() => {
  const stream = await navigator.mediaDevices.getDisplayMedia({
    video: true,
  })

  const mime = MediaRecorder.isTypeSupported('video/webm; codecs=vp9')
    ? 'video/webm; codecs=vp9'
    : 'video/webm'

  const mediaRecorder = new MediaRecorder(stream, {
    mimeType: mime,
  })

  const chunks: Array<Blob> = []
  mediaRecorder.addEventListener('dataavailable', (e) => {
    chunks.push(e.data)
  })

  mediaRecorder.addEventListener('stop', () => {
    const blob = new Blob(chunks, {
      type: chunks[0].type,
    })

    if (player.value) {
      player.value.src = URL.createObjectURL(blob)
      if (player.value.src)
        videoPath.value = player.value.src
      isShow.value = true
    }
  })

  // we have to start the recorder manually
  mediaRecorder.start()
}

watchEffect(() => {
  if (alt.value && r.value && ctrl.value)
    startShareScreen()
})

</script>
