<template>
  <header class="footer">
    <AppLogo class="logo" />
  </header>
</template>

<script>
import AppLogo from "./AppLogo.vue";

export default {
  name: "AppFooter",
  components: {
    AppLogo,
  },
};
</script>

<style scoped>
.footer {
  align-items: center;
  justify-content: space-between;
  height: 100px;
  background-color: rgba(65, 22, 127);
  margin: 0;
  width: 100%;
  padding: 0px 20px;
  box-sizing: border-box;
  margin-top: 90px;
}
</style>
