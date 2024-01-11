<template>
  <div class="container">
    <div class="header">
      <div class="tooltip">
        ?
        <span class="tooltiptext"
          >Informações sobre a Taxa de Cancelamento (Churn Rate).</span
        >
      </div>
      <h3>Taxa de Cancelamento (Churn Rate)</h3>
    </div>
    <div class="chart-container">
      <canvas id="churnChart"></canvas>
    </div>
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
.container {
  background-color: #f0f0f0;
  border-radius: 8px;
  padding: 20px;
  width: 600px;
  margin: 20px 10px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.header {
  display: flex;
  align-items: center;
  justify-content: flex-start;
  width: 100%;
}

.tooltip {
  display: inline-block;
  background-color: #fff;
  border-radius: 50%;
  text-align: center;
  width: 30px; 
  height: 30px;
  line-height: 30px;
  margin-right: 10px;
  cursor: pointer;
  font-size: 16px; 
}

.tooltip .tooltiptext {
  visibility: hidden;
  width: 160px;
  background-color: black;
  color: #fff;
  text-align: center;
  border-radius: 6px;
  padding: 5px 0;
  position: absolute;
  z-index: 1;
  bottom: 150%;
  left: 50%;
  margin-left: -80px; 
}

.tooltip:hover .tooltiptext {
  visibility: visible;
}

.chart-container {
  width: 600px;
  margin-top: 20px;
}
</style>
