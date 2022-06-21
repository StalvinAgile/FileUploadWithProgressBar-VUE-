<template>
  <div>
    <div class="input-group">
      <input type="text" :value="files.name" class="form-control" placeholder="Choose your file" />
      <span class="input-group-btn btn-border">
        <button class="btn btn-default" type="button" @click="showFilePicker">
          <v-icon>mdi-attachment</v-icon>
        </button>
      </span>
      <input type="file" ref="file" style="display: none" @change="onChange"/>
    </div>
  </div>
</template>

<script>
export default {
  data: () => ({
    files: '',
  }),
  methods: {
    showFilePicker() {
      this.$refs.file.click();
    },
    onChange(event) {
      this.files = event.target.files[0];
      this.$emit('file-change', this.files);
    },
  },
};
</script>

<style scoped>
.btn-border {
  border: 1px solid #b3b3b3;
  border-radius: 8px;
}
</style>