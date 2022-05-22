<template>
  <ReloadPWA />
  <div flex="~" justify="center">
    <img  alt="Vue logo" src="./assets/logo.png" />
  </div>
  <p>this app is pwa with vue and vite</p>

  <a-modal
    v-model:visible="visible"
    title="Basic Modal"
    @ok="handleOk"
    @cancel="visible = false"
  >
    <h4>would you like to install this app on your screen phone?</h4>
  </a-modal>
</template>

<script lang="ts" setup>
import { defineComponent, onMounted, ref } from "vue";
import ReloadPWA from "./components/ReloadPWA.vue";

const visible = ref<boolean>(false);

const handleOk = () => {
  installApp();
};

let differedPrompt = null;

function installApp() {
  visible.value = false;
  differedPrompt.prompt();
  differedPrompt.userChoice.then((choiceResult) => {
    if (choiceResult.outcome === "accepted") {
      console.log("user accepted the A2HS prompt");
    } else {
      console.log("user dismissed the A2HS prompt");
    }
    visible.value = false;
    differedPrompt = null;
  });
}

onMounted(() => {
  let isMobileCheck = /iPhone|iPad|iPod|Android/i.test(navigator.userAgent);

  window.addEventListener("beforeinstallprompt", (e) => {
    e.preventDefault();
    differedPrompt = e;
    if (isMobileCheck) {
      visible.value = true;
    }
  });
});
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
