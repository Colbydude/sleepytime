<template>
  <div id="app">
    <SleepyTime />
  </div>
</template>

<script>
import SleepyTime from './components/SleepyTime.vue'

import './assets/normalize.css';
import './assets/skeleton.css';
import './assets/app.css';

export default {
  name: 'App',
  components: {
    SleepyTime
  }
}
</script>
