<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Tab 1</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Tab 1</ion-title>
        </ion-toolbar>
      </ion-header>

      <ExploreContainer name="Tab 1 pages" />
      <button v-on:click="clicked">Click this</button>
      {{ webcams }}
      <div class="videoSlots">
        <video ref="videoRef1" autoplay="true" class="videoFrame"></video>
        <video ref="videoRef2" autoplay="true" class="videoFrame"></video>
        <video ref="videoRef3" autoplay="true" class="videoFrame"></video>
      </div>
      {{ usermedia }}
      {{ videoSettings }}
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import {
  IonPage,
  IonHeader,
  IonToolbar,
  IonTitle,
  IonContent,
} from '@ionic/vue'
import ExploreContainer from '@/components/ExploreContainer.vue'

const usermedia = ref({})

const webcams = ref([])

const videoSettings = ref([])

const videoRef1 = ref(null)

const videoRef2 = ref(null)

const videoRef3 = ref(null)

async function clicked() {
  console.log('this is in clicked')
  console.log(navigator.getUserMedia)
  const stream = await navigator.mediaDevices.getUserMedia({ video: true })
  stream.getTracks().forEach((track) => {
    track.stop()
  })
  await getDevices()
  const videos = []
  videos.push(videoRef1)
  videos.push(videoRef2)
  const deviceId = []
  let i = 0
  console.log(videos)
  // webcams.forEach(async (webcam) => {
  //   deviceId.push(webcam.deviceId)
  //   videos[i].srcObject = await navigator.mediaDevices.getUserMedia({
  //     video: true,
  //   })
  //   i = i + 1
  // })
  const stream1 = await navigator.mediaDevices.getUserMedia({
    video: { deviceId: webcams.value[0].deviceId },
  })
  videoRef1.value.srcObject = stream1
  videoSettings.value.push(stream1.getTracks()[0].getSettings().height)

  const stream2 = await navigator.mediaDevices.getUserMedia({
    video: { deviceId: webcams.value[1].deviceId },
  })
  videoRef2.value.srcObject = stream2
  videoSettings.value.push(stream2.getTracks()[0].getSettings().height)

  // const stream3 = await navigator.mediaDevices.getUserMedia({
  //   video: { deviceId: webcams.value[2].deviceId },
  // })
  // videoRef3.value.srcObject = stream3
  // videoSettings.value.push(stream3.getTracks()[0].getSettings().height)

  // console.log(webcams)
  // getDevices()
}

async function getDevices() {
  const deviceList = await navigator.mediaDevices.enumerateDevices()
  webcams.value = []
  deviceList.forEach((mediaDevice) => {
    if (mediaDevice.kind === 'videoinput') {
      webcams.value.push(mediaDevice)
      console.log(mediaDevice)
    }
  })
}
</script>

<style>
.videoFrame {
  width: 30%;
}
</style>
