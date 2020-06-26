<template>
  <div id="app">

    <banner />
    <research />
    
  </div>
</template>

<script>
import banner from './components/banner.vue'
import research from './components/research.vue'

export default {
  name: 'App',
  components: {
    research,
    banner,
  }
}
</script>

<style scoped src="@/css/test.css" />
