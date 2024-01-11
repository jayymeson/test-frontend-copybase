<template>
  <div id="app">
    <AppHeader />
    <div class="flex-container">
      <MRRChart />
      <ChurnRateChart />
      <RevenuePerCustomerChart />
      <ARPUChart />
    </div>
    <AppFooter />
    <!-- Mais conteúdo aqui, se necessário -->
  </div>
</template>

<script>
import MRRChart from "./components/MRRChart.vue";
import ChurnRateChart from "./components/ChurnRateChart.vue";
import RevenuePerCustomerChart from "./components/RevenuePerCustomersChart.vue";
import ARPUChart from "./components/ARPUChart.vue";
import AppHeader from "./components/AppHeader.vue";
import AppFooter from "./components/AppFooter.vue";

export default {
  name: "App",
  components: {
    MRRChart,
    ChurnRateChart,
    RevenuePerCustomerChart,
    ARPUChart,
    AppHeader,
    AppFooter,
  },
};
</script>

<style>
.header {
  margin-bottom: 90px;
}

.flex-container {
  display: flex;
  justify-content: center;
  gap: 20px;
  flex-wrap: wrap;
  padding: 0;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

html,
body {
  margin: 0;
  padding: 0;
  overflow-x: hidden;
}
</style>
