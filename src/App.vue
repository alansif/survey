<template>
  <div id="app">
    <el-button @click.native="startHacking">Let's do it</el-button>
        <router-view></router-view>
  </div>
</template>

<script>
export default {
  data () {
    return {
    }
  },

  methods: {
    startHacking () {
      this.$notify({
        title: 'It Works',
        message: 'We have laid the groundwork for you. Now it\'s your time to build something epic!',
        duration: 6000
      })
    }
  }
}
</script>

<style>
body {
  font-family: Helvetica, sans-serif;
}
</style>
