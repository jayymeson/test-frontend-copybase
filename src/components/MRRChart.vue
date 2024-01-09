<template>
  <div style="position: relative; height: 20vh; width: 40vw">
    <canvas id="myChart"></canvas>
  </div>
</template>

<script>
import Chart from "chart.js/auto";
import axios from "axios";

export default {
  name: "MRRChart",
  data() {
    return {
      myChart: null,
      activeSubscriptions: [],
    };
  },
  mounted() {
    this.fetchData();
  },
  methods: {
    fetchData() {
      axios
        .get("http://localhost:3000/metrics/mrr?year=2022")
        .then((response) => {
          this.activeSubscriptions = response?.data.map(
            (item) => item?.activeSubscriptions
          );
          this.createChart(response.data);
        })
        .catch((error) => {
          console.error("Error fetching data:", error);
        });
    },
    createChart(chartData) {
      const months = chartData?.map((item) => this.translateMonth(item.month));
      const mrrValues = chartData?.map((item) => item.totalMRR);

      const ctx = document.getElementById("myChart");
      const config = {
        type: "bar",
        data: {
          labels: months,
          datasets: [
            {
              label: "MRR",
              data: mrrValues,
              backgroundColor: ["rgba(65, 22, 127)"],
              borderColor: ["rgba(65, 22, 127)"],
              borderWidth: 1,
            },
          ],
        },
        options: {
          scales: {
            y: {
              beginAtZero: true,
            },
          },
          tooltips: {
            callbacks: {
              label: (tooltipItem) => {
                return `MRR: ${tooltipItem?.yLabel.toLocaleString()} - Assinaturas: ${
                  this.activeSubscriptions[tooltipItem?.index]
                }`;
              },
            },
          },
        },
      };

      if (this.myChart) {
        this.myChart.destroy();
      }
      this.myChart = new Chart(ctx, config);
    },
    translateMonth(month) {
      const monthTranslations = {
        January: "Janeiro",
        February: "Fevereiro",
        March: "Março",
        April: "Abril",
        May: "Maio",
        June: "Junho",
        July: "Julho",
        August: "Agosto",
        September: "Setembro",
        October: "Outubro",
        November: "Novembro",
        December: "Dezembro",
      };
      return monthTranslations[month] || month;
    },
  },
};
</script>

<style scoped>
/* Seus estilos */
</style>
