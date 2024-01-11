<template>
  <div id="app">
    <div class="flex-container">
      <MRRChart />
      <ChurnRateChart />
      <RevenuePerCustomerChart />
      <ARPUChart />
    </div>
    <!-- Mais conteúdo aqui, se necessário -->
  </div>
</template>

<script>
import MRRChart from "./components/MRRChart.vue";
import ChurnRateChart from "./components/ChurnRateChart.vue";
import RevenuePerCustomerChart from "./components/RevenuePerCustomersChart.vue";
import ARPUChart from "./components/ARPUChart.vue";

export default {
  name: "App",
  components: {
    MRRChart,
    ChurnRateChart,
    RevenuePerCustomerChart,
    ARPUChart,
  },
};
</script>

<style>
.flex-container {
  display: flex;
  justify-content: center;
  gap: 20px;
  flex-wrap: wrap;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
