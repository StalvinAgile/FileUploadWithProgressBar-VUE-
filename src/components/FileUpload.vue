<template>
  <div class="container">
    <section id="image-form-wrapper">
      <div class="container mt-5">
        <div class="row">
          <div class="col-md-6 col-md-offset-3">
            <div class="panel panel-default">
              <v-card class="p-2 text-center">
                <div class="panel-heading">
                  <h5 class="panel-title">Upload Your File</h5>
                </div>
              </v-card>
              <v-card class="py-3 px-2 mt-1">
                <form @submit.prevent="handleSubmit">
                  <div class="form-group">
                    <FileInput v-on:file-change="setFiles"> </FileInput>
                  </div>
                  <div class="form-group mt-2">
                    <button
                      type="submit"
                      class="btn btn-success"
                      :disabled="!files"
                    >
                      Upload
                    </button>
                  </div>
                  <div class="mt-3">
                  <ProgressBar :progress="progress"  v-if="isUploading"></ProgressBar>
                  </div>
                </form>
              </v-card>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import FileInput from "./FileInput.vue";
import ProgressBar from "./ProgressBar.vue";
import axios from "axios";
export default {
  components: { FileInput, ProgressBar },
  data: () => ({
    files: "",
    progress: 0,
     isUploading:false,
  }),


  methods: {
    handleSubmit() {
      this.isUploading=true;
      let formData = new FormData();
      formData.append("file_name", this.files);
      axios
        .post(process.env.VUE_APP_API_URL_ADMIN + "store-image", formData,{
          onUploadProgress: (e) => {
            if (e.lengthComputable) {
              this.progress = (e.loaded / e.total) * 100;
            }
          }
        })
        .then((res) => {
          console.log("upload completed");
          setTimeout(() => {
            this.isUploading=false;
            this.progress = 0;
          }, 1500);

        })
        .catch((err) => {
          console.log("upload failed");
        });
    },
    setFiles(files) {
      if (files != undefined) {
        this.files = files;
      }
    },
  },
};
</script>

<style>
</style>