<template>
  <v-app>
    <v-card class="pt-3 pb-1 px-2 mt-1">
      <form @submit.prevent="handleSubmit">
        <div class="form-group">
          <FileInput v-on:file-change="setFiles"> </FileInput>
        </div>
        <div class="form-group mt-2 d-flex justify-content-end">
          <v-tooltip bottom>
            <template v-slot:activator="{ on }">
              <a v-on="on">
                <v-btn
                  v-if="filepath"
                  class="mr-2"
                  small
                  type="submit"
                  :disabled="!files || disableBtn"
                >
                  <a
                    class="text-decoration-none"
                    target="_blank"
                    :href="env_path + filepath"
                  >
                    Download
                  </a>
                </v-btn>
              </a>
            </template>
            <span>Download</span>
          </v-tooltip>
          <v-tooltip bottom>
            <template v-slot:activator="{ on }">
              <a v-on="on">
                <v-btn
                  color="success"
                  small
                  type="submit"
                  :disabled="!files || disableBtn"
                >
                  Upload
                </v-btn>
              </a>
            </template>
            <span>Upload</span>
          </v-tooltip>
        </div>
        <div class="mt-3">
          <ProgressBar :progress="progress" v-if="isUploading"></ProgressBar>
        </div>
      </form>
      <!-- <a
          target="_blank"
          v-if="filepath"
          class="ml-2 mt-2"
          :href="env_path + filepath"
          >Download</a
        > -->
      <!-- <img v-if="filepath" :src='env_path + filepath' alt="img" width="200" class="image-preview"> -->
    </v-card>
  </v-app>
</template>

<script>
import FileInput from "./FileInput.vue";
import ProgressBar from "./ProgressBar.vue";
import axios from "axios";
export default {
  props: {
    folderName: {
      type: String,
      default: "",
    },
  },

  components: { FileInput, ProgressBar },
  data: () => ({
    filepath: "",
    env_path: process.env.VUE_APP_FILE_ADMIN,
    files: "",
    disableBtn: false,
    progress: 0,
    isUploading: false,
  }),

  methods: {
    handleSubmit() {
      this.disableBtn = true;
      this.isUploading = true;
      let formData = new FormData();
      formData.append("file_name", this.files);
      formData.append("folder_name", this.folderName);
      axios
        .post(process.env.VUE_APP_API_URL_ADMIN + "store-image", formData, {
          onUploadProgress: (e) => {
            if (e.lengthComputable) {
              this.progress = (e.loaded / e.total) * 100;
            }
          },
        })
        .then((res) => {
          // console.log("response ->", res.data);
          this.filepath = res.data.filepath.file_name;
          setTimeout(() => {
            this.isUploading = false;
            this.progress = 0;
            this.disableBtn = false;
          }, 1500);
        })
        .catch((err) => {
          console.log(err);
          this.isUploading = false;
          this.progress = 0;
          this.disableBtn = false;
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

<style scoped>
.image-preview {
  border: 2px double black;
  padding: 2px;
}
</style>