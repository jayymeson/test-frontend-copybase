<template>
  <header class="header">
    <AppLogo class="logo" />
    <nav class="navigation"></nav>
    <button v-if="isLoggedIn" class="action-button" @click="showUploadModal">
      Upload Planilha
    </button>
    <button v-else class="action-button" @click="showLoginModal">Login</button>
    <LoginModal ref="loginModal" @login-success="handleLoginSuccess" />
    <UploadModal ref="uploadModal" @upload-success="$emit('upload-success')" />
  </header>
</template>

<script>
import AppLogo from "./AppLogo.vue";
import LoginModal from "./LoginModal.vue";
import UploadModal from "./UploadModal.vue";

export default {
  name: "AppHeader",
  components: {
    AppLogo,
    LoginModal,
    UploadModal,
  },
  data() {
    return {
      isLoggedIn: !!localStorage.getItem("token"),
    };
  },
  methods: {
    showLoginModal() {
      this.$refs.loginModal.isVisible = true;
    },
    showUploadModal() {
      this.$refs.uploadModal.isVisible = true;
    },
    handleLoginSuccess() {
      this.isLoggedIn = true;
    },
    updateCharts() {
      this.$refs.mrrChart.fetchData();
      this.$refs.churnRateChart.fetchChurnData();
      this.$refs.arpuChart.fetchData();
      this.$refs.revenuePerCustomerChart.fetchData();
    },
  },
};
</script>

<style scoped>
.header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: 100px;
  background-color: rgba(65, 22, 127);
  margin: 0;
  width: 100%;
  padding: 00px 20px;
  box-sizing: border-box;
  margin-bottom: 90px;
}

.navigation {
  display: flex;
  margin-left: auto;
}

.action-button {
  padding: 10px 20px;
  background-color: #5c4baf;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 1rem;
  margin-left: 20px;
}
</style>
