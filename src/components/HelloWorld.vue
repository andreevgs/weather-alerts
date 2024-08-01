<script setup lang="ts">

defineProps<{ msg: string }>()

function registerBackgroundSync() {
  if (!navigator.serviceWorker){
    return console.error("Service Worker not supported")
  }

  navigator.serviceWorker.ready
      .then(registration => {(registration as any).periodicSync.register('periodicSync', {minInterval: 1000}); console.log(registration)})
      .then(() => console.log("Registered background sync"))
      .catch(err => console.error("Error registering background sync", err))
}

function unregisterBackgroundSync() {
  if (!navigator.serviceWorker) {
    return console.error("Service Worker not supported");
  }

  navigator.serviceWorker.ready
      .then(registration => {
        registration.unregister()
          return (registration as any).periodicSync.unregister('syncAttendees');
        })
      .then(() => console.log("Unregistered background sync"))
      .catch(err => console.error("Error unregistering background sync", err));
}

function registerNotification() {
  Notification.requestPermission(notificationPermission => {
    if (notificationPermission === 'granted'){
      navigator.permissions.query({
        // @ts-ignore
        name: 'periodic-background-sync',
      }).then(backgroundSyncPermission => {
        console.log(backgroundSyncPermission)
        if(backgroundSyncPermission.state === 'granted'){
          registerBackgroundSync()
        }
      });
    }
    else console.error("Permission was not granted.")
  })
}

</script>

<template>
  <h1>{{ msg }}</h1>

  <div class="card">
    <button type="button" @click="registerNotification()">Notify</button>
    <button type="button" @click="unregisterBackgroundSync()">Stop</button>
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
