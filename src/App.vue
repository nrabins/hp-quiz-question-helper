<template>
  <div id="app">
    <v-input/>
    <v-output/>
  </div>
</template>

<script>
import Input from '@/components/Input';
import Output from '@/components/Output';

export default {
  name: 'app',
  components: {
    'v-input': Input,
    'v-output': Output
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
