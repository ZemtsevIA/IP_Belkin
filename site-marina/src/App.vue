<template>
  <div id="app">
    <router-view />
  </div>
</template>

<script>
export default {
  name: 'App'
};
</script>

<style>
#app {
  width: 100%;
  min-height: 100vh;
  
}
@import '@/style.css'; /* Импортируем style.css */
</style>