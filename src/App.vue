<template>
  <div id="app container">
    <div class="file-upload m-5">
      <FileUpload :folderName="'Excel'" />
    </div>
  </div>
</template>

<script>
import FileUpload from "./components/FileUpload.vue";

export default {
  name: "app",
  components: {
    FileUpload,
  },
};
</script>

<style scoped>
.file-upload{
  width: fit-content;
}
</style>
