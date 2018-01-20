<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'app',
};
</script>

<style lang="stylus" scoped>
#app {
  background #FFF;
  color: #000;
  height: 100%;
  font-family: 'Helvetica Neue', Helvetica, 'Microsoft Yahei', Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
