<template>
  <div id="app">
    <AppHeader @upload-success="updateCharts" />
    <div class="flex-container">
      <MRRChart ref="mrrChart" />
      <ChurnRateChart ref="churnRateChart" />
      <RevenuePerCustomerChart ref="revenuePerCustomerChart" />
      <ARPUChart ref="arpuChart" />
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

  methods: {
    updateCharts() {
      if (this.$refs.mrrChart) this.$refs.mrrChart.fetchData();
      if (this.$refs.churnRateChart) this.$refs.churnRateChart.fetchChurnData();
      if (this.$refs.arpuChart) this.$refs.arpuChart.fetchData();
      if (this.$refs.revenuePerCustomerChart)
        this.$refs.revenuePerCustomerChart.fetchData();
    },
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
