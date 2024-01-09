<template>
  <div style="position: relative; height: 40vh; width: 80vw">
    <canvas id="churnChart"></canvas>
  </div>
</template>

<script>
import Chart from "chart.js/auto";
import axios from "axios";

export default {
  name: "ChurnRateChart",
  data() {
    return {
      churnChart: null,
      churnRateData: [],
      cancellationsData: [],
    };
  },
  mounted() {
    this.fetchChurnData();
  },
  methods: {
    async fetchChurnData() {
      try {
        const response = await axios.get(
          "http://localhost:3000/metrics/churn-rate?year=2022"
        );
        this.churnRateData = response.data.map((item) => item.churnRate);
        this.cancellationsData = response.data.map(
          (item) => item.cancellations
        );
        this.createChurnChart();
      } catch (error) {
        console.error("Error fetching churn data:", error);
      }
    },
    createChurnChart() {
      const months = [
        "Janeiro",
        "Fevereiro",
        "Março",
        "Abril",
        "Maio",
        "Junho",
        "Julho",
        "Agosto",
        "Setembro",
        "Outubro",
        "Novembro",
        "Dezembro",
      ];

      const ctx = document.getElementById("churnChart");
      const config = {
        type: "line",
        data: {
          labels: months,
          datasets: [
            {
              label: "Churn Rate",
              data: this.churnRateData,
              fill: false,
              backgroundColor: ["rgba(65, 22, 127)"],
              borderColor: ["rgba(65, 22, 127)"],
              tension: 0.1,
            },
          ],
        },
        options: {
          scales: {
            y: {
              beginAtZero: true,
            },
          },
          plugins: {
            legend: {
              position: "bottom",
            },
            tooltip: {
              callbacks: {
                label: (context) => {
                  const churnRate = context.parsed.y.toFixed(2);
                  const cancellations =
                    this.cancellationsData[context.dataIndex];
                  return `Churn Rate: ${churnRate}% - Cancelamentos: ${
                    cancellations ?? "N/A"
                  }`;
                },
              },
            },
          },
        },
      };

      if (this.churnChart) {
        this.churnChart.destroy();
      }
      this.churnChart = new Chart(ctx, config);
    },
  },
};
</script>

<style scoped>
/* Seus estilos */
</style>
