<script setup lang="ts">
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
import HelloWorld from './components/HelloWorld.vue';
import 'ziggeo-client-sdk/build/ziggeo';

let recorder;
let params = {
  appToken: 'r173a2165cb3c4091f20968456ec1b01',
  effectProfiles: [],
  expirationDays: 1000,
  metaProfiles: 'r1784b9efef62ec27ff1927ecf9d1009',
  transcriptionLanguage: 'en-GB',
  uploadAuthToken: 'r1b3442d6215ec31c03e7d23ff7cbbf3',
  videoProfiles: ['r10a54709a73f864ed122751fd4ea7da'],
};

const application = ZiggeoApi.V2.Application.instanceByToken(params.appToken, {
  auth: true,
  debug: true,
  webrtc_streaming_if_necessary: true,
  webrtc_on_mobile: true,
});

console.log(ZiggeoApi, application);

application.on('ready', () => {
  console.log('ready');

  const divCnt = document.createElement('div');
  document.body.appendChild(divCnt);

  recorder = new ZiggeoApi.V2.Recorder({
    element: divCnt,
    attrs: {
      'server-auth': params.uploadAuthToken,
      'expiration-days': params.expirationDays,
      'transcript-language': params.transcriptionLanguage || 'en-US',
      'meta-profile': params.metaProfiles.toString(),
      'effect-profile': params.effectProfiles,
      'video-profile': params.videoProfiles.toString(),
      picksnapshots: false,
      framerate: params.frameRate,
      'custom-covershots': false,
      'default-image-selector': '0.0002',
      recordingwidth: params.recordingWidth,
      recordingheight: params.recordingHeight,
      width: params.recordingWidth,
      height: params.recordingHeight,
    },
  });

  recorder.activate();
});
</script>

<template>
  <div>
    <a href="https://vitejs.dev" target="_blank">
      <img src="/vite.svg" class="logo" alt="Vite logo" />
    </a>
    <a href="https://vuejs.org/" target="_blank">
      <img src="./assets/vue.svg" class="logo vue" alt="Vue logo" />
    </a>
  </div>
  <HelloWorld msg="Vite + Vue" />
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
