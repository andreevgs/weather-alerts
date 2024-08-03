<script setup lang="ts">
import axios from "axios";

defineProps<{ msg: string }>();

function subscribeNotifications() {
  if (!navigator.serviceWorker) {
    return alert("Service Worker not supported");
  }

  navigator.serviceWorker.ready.then((registration) => {
    registration.pushManager
      .subscribe({
        userVisibleOnly: true,
        applicationServerKey: urlBase64ToUint8Array(
          "BJ2TBvZx9d95NVO_lBj4uekE3mhaZJAJH4JX4BG98lcM4EvPNIE8JTuNC3cVwZ-yg1m5N4E-b6m9QfUi6-NVx-c",
        ),
      })
      .then((subscription) => {
        alert(JSON.stringify(subscription));
        axios
          .post(
            "https://grateful-catherin-weather-alerts-58ee34dc.koyeb.app/subscribe/",
            subscription,
            {
              headers: {
                "Content-Type": "application/json",
              },
            },
          )
          .catch((error) => alert(JSON.stringify(error)));
      });
  });
}

function urlBase64ToUint8Array(base64String: string) {
  const padding = "=".repeat((4 - (base64String.length % 4)) % 4);
  const base64 = (base64String + padding).replace(/-/g, "+").replace(/_/g, "/");
  const rawData = window.atob(base64);
  const outputArray = new Uint8Array(rawData.length);
  for (let i = 0; i < rawData.length; ++i) {
    outputArray[i] = rawData.charCodeAt(i);
  }
  return outputArray;
}
</script>

<template>
  <h1>{{ msg }}</h1>

  <div class="card">
    <button type="button" @click="subscribeNotifications()">Notify</button>
    <p>
      Edit
      <code>components/HelloWorld.vue</code> to test HMR
    </p>
  </div>

  <p>
    Check out
    <a href="https://vuejs.org/guide/quick-start.html#local" target="_blank"
      >create-vue</a
    >, the official Vue + Vite starter
  </p>
  <p>
    Install
    <a href="https://github.com/vuejs/language-tools" target="_blank">Volar</a>
    in your IDE for a better DX
  </p>
  <p class="read-the-docs">Click on the Vite and Vue logos to learn more</p>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
