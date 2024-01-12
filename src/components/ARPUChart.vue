<template>
  <div class="container">
    <div class="header">
      <div class="tooltip">
        ?
        <span class="tooltiptext"
          >ARPU é a média de receita gerada por usuário ou cliente em um
          determinado período.
        </span>
      </div>
      <h3>Receita Média por Usuário (ARPU)</h3>
    </div>
    <div class="chart-container">
      <canvas id="arpuChart"></canvas>
    </div>
  </div>
</template>

<script>
import Chart from "chart.js/auto";
import axios from "axios";

export default {
  name: "ARPUChart",
  data() {
    return {
      arpuChart: null,
    };
  },
  mounted() {
    this.fetchData();
  },
  methods: {
    fetchData() {
      axios
        .get("http://localhost:3000/metrics/arpu?year=2022")
        .then((response) => {
          const months = response.data.map((item) =>
            this.translateMonth(item.month)
          );
          const arpuData = response.data.map((item) => item.ARPU);
          this.createChart(months, arpuData);
        })
        .catch((error) => {
          console.error("Error fetching ARPU data:", error);
        });
    },
    createChart(labels, data) {
      const ctx = document.getElementById("arpuChart");
      const config = {
        type: "doughnut",
        data: {
          labels: labels,
          datasets: [
            {
              label: "ARPU",
              data: data,
              backgroundColor: [
                "rgb(255, 99, 132)",
                "rgb(54, 162, 235)",
                "rgb(255, 205, 86)",
                "rgb(75, 192, 192)",
                "rgb(153, 102, 255)",
                "rgb(255, 159, 64)",
                "rgb(201, 203, 207)",
                "rgb(233, 30, 99)",
                "rgb(0, 188, 212)",
                "rgb(76, 175, 80)",
                "rgb(96, 125, 139)",
                "rgb(255, 87, 34)",
              ],
              hoverOffset: 4,
            },
          ],
        },
        options: {
          plugins: {
            legend: {
              position: "left",
              align: "left",
              labels: {
                boxWidth: 20,
                padding: 10,
              },
              generateLabels: (chart) => {
                const original =
                  Chart.overrides.doughnut.plugins.legend.labels.generateLabels;
                const labelsOriginal = original.call(this, chart);
                labelsOriginal.forEach((label) => {
                  label.textStyle = "legend-style";
                });
                return labelsOriginal;
              },
              tooltip: {
                callbacks: {
                  label: (context) => {
                    return `${
                      context.label
                    }: R$${context.parsed.toLocaleString()}`;
                  },
                },
              },
            },
          },
        },
      };

      if (this.arpuChart) {
        this.arpuChart.destroy();
      }
      this.arpuChart = new Chart(ctx, config);
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
  margin-bottom: 30px;
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
  position: relative;
  display: inline-block;
  cursor: pointer;
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

.legend-style {
  position: absolute;
  left: 0;
  top: 50%;
  transform: translateY(-50%);
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.chart-container .legend-style ul {
  position: absolute;
  left: 0;
  top: 50%;
  transform: translateY(-50%);
  padding-left: 0;
  list-style: none;
  margin: 0;
}

.chart-container {
  position: relative;
  width: 400px;
  margin-top: 20px;
}
</style>
