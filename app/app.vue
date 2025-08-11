<script setup>
const route = useRoute()

onMounted(async () => {
  if (import.meta.client) {
    const mpegts = await import('mpegts.js')

    if (mpegts.getFeatureList().mseLivePlayback) {
      const videoElement = document.getElementById('videoElement')

      const player = mpegts.createPlayer({
        type: 'flv',
        isLive: true,
        liveSync: true,
        enableStashBuffer: false,
        url: route.query.url,
      })

      player.attachMediaElement(videoElement)
      player.load()
      player.play()
    }
  }
})
</script>

<template>
  <video id="videoElement" autoplay controls muted class="w-screen h-screen" />
</template>

<style>
body {
  font-family: var(--font-display);
}
</style>
