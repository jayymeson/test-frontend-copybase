<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png" /> -->
    <MRRChart />
    <!-- <churn-rate-chart :month="selectedMonth" /> -->
  </div>
</template>

<script>
import MRRChart from "./components/MRRChart.vue";
// import ChurnRateChart from "./components/ChurnRateChart.vue";

export default {
  name: "App",
  components: {
    MRRChart,
    // ChurnRateChart,
  },
  // data() {
  //   return {
  //     selectedMonth: "2022-11",
  //   };
  // },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
