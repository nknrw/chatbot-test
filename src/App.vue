<template>
  <div id="app">
    <ChatWidget />
  </div>
</template>

<script>
import ChatWidget from './components/ChatWidget.vue';

export default {
  name: 'App',
  components: {
    ChatWidget
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto&display=swap');
  
#app {
  text-align: center;
  color: #000000;
  margin: 0;
  padding: 0;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}

html {
  height: 100%;
  margin: 0;
  padding: 0;
  user-select: none;
}

body {
  margin: 0;
  padding: 0;
  height: 100%;
  overflow: hidden;
  font-family: 'Roboto', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  background-color: #f0f0f0;
}

</style>
