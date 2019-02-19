<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style lang='less'>
h1,
body,
html,
div,
ul,
li,
input,
button {
  margin: 0;
  padding: 0;
}
body {
  font: 14px "Helvetica Neue", Helvetica, Arial, sans-serif;
  background: #f5f5f5;
  color: #4d4d4d;
  margin: 0 auto;
  font-weight: 300;
}
</style>
