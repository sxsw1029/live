<script setup>
const route = useRoute()
const videoElement = ref(null)
const plyrPlayer = ref(null)
const mpegtsPlayer = ref(null)

onMounted(async () => {
  if (import.meta.client) {
    const Plyr = (await import('plyr')).default
    const mpegts = (await import('mpegts.js')).default

    plyrPlayer.value = new Plyr(videoElement.value, {
      autoplay: true,
    })

    if (mpegts.getFeatureList().mseLivePlayback) {
      mpegtsPlayer.value = mpegts.createPlayer({
        type: 'flv',
        isLive: true,
        liveSync: true,
        enableStashBuffer: false,
        url: route.query.url,
      })

      mpegtsPlayer.value.attachMediaElement(videoElement.value)
      mpegtsPlayer.value.load()
      mpegtsPlayer.value.play()
    }
  }
})
</script>

<template>
  <video v-show="plyrPlayer" ref="videoElement" controls crossorigin playsinline autoplay muted class="min-h-screen max-h-screen" />
</template>

<style>
@import "plyr/dist/plyr.css";

body {
  font-family: var(--font-display);
}
</style>
