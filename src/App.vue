<template>
  <div id="app">
    <FileUpload :folderName="'Excel'"/>
  </div>
</template>

<script>
import FileUpload from './components/FileUpload.vue'

export default {
  name: 'app',
  components: {
    FileUpload
  }
}
</script>

<style>

</style>
