<template>
  <div class="modal" v-if="isVisible">
    <div class="modal-content">
      <span class="close" @click="closeModal">&times;</span>
      <h2>Upload de Planilha</h2>
      <div class="input-file-container">
        <label class="input-file-trigger" for="fileInput"
          >Escolher arquivo</label
        >
        <input
          type="file"
          id="fileInput"
          @change="fileSelected"
          accept=".xlsx"
          required
          class="input-file"
        />
        <span class="file-return" ref="fileReturn"
          >Nenhum arquivo escolhido</span
        >
      </div>
      <button type="submit" @click="uploadFile" :disabled="isLoading">
        UPLOAD
      </button>
      <div v-if="fileError" class="error-message">{{ fileError }}</div>
      <div v-if="isLoading" class="loading-overlay">
        <div class="loader"></div>
      </div>
      <div v-if="uploadResponse" class="upload-info">
        <p>{{ uploadResponse.message }}</p>
        <p>Registros inseridos: {{ uploadResponse.insertedRecords }}</p>
        <p>Registros ignorados: {{ uploadResponse.ignoredRecords }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "UploadModal",
  data() {
    return {
      isVisible: false,
      selectedFile: null,
      fileError: "",
      uploadResponse: null,
      isLoading: false,
    };
  },
  methods: {
    closeModal() {
      this.isVisible = false;
    },
    fileSelected(event) {
      const file = event.target.files[0];
      if (
        file &&
        file.type ===
          "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet"
      ) {
        this.selectedFile = file;
        this.fileError = "";
        this.$refs.fileReturn.textContent = file.name;
      } else {
        this.selectedFile = null;
        this.fileError =
          "Formato não aceito. Por favor, envie um arquivo .xlsx.";
      }
    },
    uploadFile() {
      if (!this.selectedFile) {
        this.fileError = "Por favor, selecione um arquivo para upload.";
        return;
      }
      this.isLoading = true;
      const formData = new FormData();
      formData.append("file", this.selectedFile);

      const token = localStorage.getItem("token");

      axios
        .post("http://localhost:3000/subscription/upload", formData, {
          headers: {
            Authorization: `Bearer ${token}`,
          },
        })
        .then((response) => {
          if (response.status === 201) {
            this.uploadResponse = {
              message: "Upload processado!",
              insertedRecords: response.data.insertedRecords,
              ignoredRecords: response.data.ignoredRecords,
            };
            this.$emit("upload-success");
          } else {
            this.fileError =
              "Ocorreu um erro durante o upload. Por favor, tente novamente.";
          }
        })
        .catch((error) => {
          this.fileError = "Falha ao realizar o upload: " + error.message;
          console.error("Erro no upload:", error);
        })
        .finally(() => {
          this.isLoading = false;
        });
    },
  },
};
</script>

<style scoped>
.modal {
  display: flex;
  position: fixed;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  justify-content: center;
  align-items: center;
}

.modal-content {
  background: white;
  padding: 20px;
  border-radius: 5px;
  width: 500px;
  text-align: center;
}

.close {
  float: right;
  font-size: 28px;
  cursor: pointer;
}

input[type="file"] {
  margin: 10px 0;
  display: block;
  color: white;
  background-color: rgba(65, 22, 127, 0.8);
  border-radius: 5px;
  border: none;
  text-align: center;
  cursor: pointer;
  outline: none;
}

input[type="file"]::-webkit-file-upload-button {
  visibility: hidden;
}

input[type="file"]::before {
  content: "Escolher arquivo";
  display: inline-block;
  background-color: rgba(65, 22, 127, 0.8);
  border: none;
  border-radius: 5px;
  padding: 10px 20px;
  outline: none;
  white-space: nowrap;
  cursor: pointer;
  text-align: center;
  font-weight: 700;
  color: white;
}

input[type="file"]:hover::before {
  background-color: rgba(65, 22, 127);
}

input[type="file"]:active::before {
  background-color: rgba(65, 22, 127);
}

.input-file-container {
  position: relative;
  text-align: left;
  margin: 10px 0;
}

.input-file-trigger {
  padding: 10px 20px;
  background-color: rgba(65, 22, 127, 0.8);
  color: white;
  border-radius: 5px;
  cursor: pointer;
  font-weight: 700;
  margin: 10px auto;
  width: fit-content;
}

.input-file {
  opacity: 0;
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  left: 0;
  cursor: pointer;
}

.file-return {
  display: block;
  margin: 10px auto;
}

button {
  background-color: rgba(65, 22, 127);
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  display: block;
  margin: 10px auto;
  margin-left: 0;
}

.upload-info {
  text-align: left;
  margin-top: 20px;
}

.loading-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(255, 255, 255, 0.7);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 10;
}

.loader {
  border: 5px solid #f3f3f3;
  border-top: 5px solid rgba(65, 22, 127, 0.8);
  border-radius: 50%;
  width: 50px;
  height: 50px;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>
