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
      <video ref="videoRef" autoplay="true" class="videoFrame"></video>

      {{ usermedia }}
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import {
  IonPage,
  IonHeader,
  IonToolbar,
  IonTitle,
  IonContent,
} from '@ionic/vue'
import ExploreContainer from '@/components/ExploreContainer.vue'

export default defineComponent({
  name: 'Tab1Page',
  components: {
    ExploreContainer,
    IonHeader,
    IonToolbar,
    IonTitle,
    IonContent,
    IonPage,
  },
  data: function () {
    return {
      usermedia: {},
    }
  },
  methods: {
    async clicked() {
      console.log('this is in clicked')
      console.log(navigator.mediaDevices.getUserMedia)
      const stream = await navigator.mediaDevices.getUserMedia({ video: {width:2560,height:1440} })
      const webcams = await navigator.mediaDevices.enumerateDevices();
      (this.$refs.videoRef as HTMLMediaElement).srcObject=stream
      console.log(webcams)
      console.log(stream)
    },
  },
})
</script>

<style>
.videoFrame{
  height:400px;
}
</style>
