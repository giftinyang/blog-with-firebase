<template>
  <div id="app">
    <app-addblog/>
  </div>
</template>

<script>
import AddBlog from '@/components/AddBlog.vue'

export default {
  name: 'app',
  components: {
    'app-addblog': AddBlog
  }
}
</script>
<style>
 
</style>
