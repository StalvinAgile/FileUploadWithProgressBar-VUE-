<template>
  <v-app>
    <div class="container">
      <section id="image-form-wrapper">
        <div class="container mt-5">
          <div class="row">
            <div class="col-md-6 col-md-offset-3">
              <div class="panel panel-default">
                <!-- <v-card class="p-2 text-center">
                <div class="panel-heading">
                  <h5 class="panel-title">Upload Your File</h5>
                </div>
              </v-card> -->
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
                            color="success"
                            
                            type="submit"
                            :disabled="!files || disableBtn"
                          >
                            Upload
                          </v-btn>
                          </a>
                        </template>
                        <span>Upload File</span>
                      </v-tooltip>
                    </div>
                    <div class="mt-3">
                      <ProgressBar
                        :progress="progress"
                        v-if="isUploading"
                      ></ProgressBar>
                    </div>
                  </form>
                </v-card>
              </div>
            </div>
          </div>
        </div>
      </section>
    </div>
  </v-app>
</template>

<script>
import FileInput from "./FileInput.vue";
import ProgressBar from "./ProgressBar.vue";
import axios from "axios";
export default {
  components: { FileInput, ProgressBar },
  data: () => ({
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
      axios
        .post(process.env.VUE_APP_API_URL_ADMIN + "store-image", formData, {
          onUploadProgress: (e) => {
            if (e.lengthComputable) {
              this.progress = (e.loaded / e.total) * 100;
            }
          },
        })
        .then((res) => {
          console.log("upload completed");
          setTimeout(() => {
            this.isUploading = false;
            this.progress = 0;
            this.disableBtn = false;
          }, 1500);
        })
        .catch((err) => {
          this.isUploading = false;
          this.progress = 0;
          this.disableBtn = false;
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