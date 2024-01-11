<template>
  <div class="container">
    <div class="header">
      <div class="tooltip">
        ?
        <span class="tooltiptext"
          >Informações sobre a Receita Mensal Recorrente (MRR).</span
        >
      </div>
      <h3>Receita Mensal Recorrente (MRR)</h3>
    </div>
    <div class="chart-container">
      <canvas id="myChart"></canvas>
    </div>
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
          this.activeSubscriptions = response.data.map(
            (item) => item.activeSubscriptions
          );
          this.createChart(response.data);
        })
        .catch((error) => {
          console.error("Error fetching data:", error);
        });
    },

    createChart(chartData) {
      const months = chartData?.map((item) => this.translateMonth(item.month));
      const mrrValues = chartData?.map((item) =>
        parseFloat(item.totalMRR.toFixed(2))
      );

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
          plugins: {
            legend: {
              position: "bottom", // Posiciona a legenda na parte inferior
            },
            tooltip: {
              callbacks: {
                label: (context) => {
                  const mrr = parseFloat(
                    context.parsed.y.toFixed(2)
                  ).toLocaleString();
                  const subscriptions =
                    this.activeSubscriptions[context.dataIndex];
                  return `R$${mrr}\n Assinaturas: ${subscriptions ?? "N/A"}`;
                },
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
  width: 30px; /* Aumente o tamanho */
  height: 30px; /* Aumente o tamanho */
  line-height: 30px; /* Ajuste para centralizar verticalmente */
  margin-right: 10px;
  cursor: pointer;
  font-size: 16px; /* Aumente o tamanho do texto */
}

.tooltip .tooltiptext {
  visibility: hidden;
  width: 160px; /* Ajuste a largura conforme necessário */
  background-color: black;
  color: #fff;
  text-align: center;
  border-radius: 6px;
  padding: 5px 0;
  position: absolute;
  z-index: 1;
  bottom: 150%; /* Ajuste a posição conforme necessário */
  left: 50%;
  margin-left: -80px; /* Ajuste conforme a largura */
}

.tooltip:hover .tooltiptext {
  visibility: visible;
}

.chart-container {
  width: 600px;
  margin-top: 20px;
}
</style>
