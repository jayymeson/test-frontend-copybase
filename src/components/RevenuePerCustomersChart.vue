<template>
  <div class="container">
    <div class="header">
      <div class="tooltip">
        ?
        <span class="tooltiptext"
          >Informações sobre a receita por cliente.</span
        >
      </div>
      <h3>Receita por Cliente</h3>
    </div>
    <div class="chart-container">
      <canvas id="revenueChart"></canvas>
    </div>
  </div>
</template>

<script>
import Chart from "chart.js/auto";
import axios from "axios";

export default {
  name: "RevenuePerCustomerChart",
  data() {
    return {
      revenueChart: null,
      topCustomersData: [],
    };
  },
  mounted() {
    this.fetchData();
  },
  methods: {
    fetchData() {
      axios
        .get("http://localhost:3000/metrics/revenue-per-customer?year=2022")
        .then((response) => {
          const topCustomers = response.data
            .sort((a, b) => b.revenue - a.revenue)
            .slice(0, 12);

          this.topCustomersData = topCustomers;
          this.createChart();
        })
        .catch((error) => {
          console.error("Error fetching data:", error);
        });
    },

    createChart() {
      const ctx = document.getElementById("revenueChart");
      const config = {
        type: "doughnut",
        data: {
          labels: this.topCustomersData.map((customer) => customer.user),
          datasets: [
            {
              label: "Revenue per Customer",
              data: this.topCustomersData.map((customer) => customer.revenue),
              backgroundColor: [
                "rgb(102, 204, 255)",
                "rgb(255, 153, 204)",
                "rgb(204, 204, 0)",
                "rgb(102, 153, 0)",
                "rgb(153, 51, 102)",
                "rgb(102, 102, 153)",
                "rgb(204, 102, 0)",
                "rgb(0, 102, 204)",
                "rgb(0, 153, 153)",
                "rgb(153, 0, 0)",
                "rgb(0, 102, 0)",
                "rgb(102, 51, 0)",
              ],
              hoverOffset: 4,
            },
          ],
        },
        options: {
          plugins: {
            legend: {
              position: "left",
              labels: {
                boxWidth: 20,
                padding: 10,
              },
            },
            tooltip: {
              callbacks: {
                label: (context) => {
                  const customer = this.topCustomersData[context.dataIndex];
                  return `${
                    customer.user
                  }: R$${customer.revenue.toLocaleString()} - Compras: ${
                    customer.purchases
                  }`;
                },
              },
            },
          },
        },
      };

      if (this.revenueChart) {
        this.revenueChart.destroy();
      }
      this.revenueChart = new Chart(ctx, config);
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
  width: 400px;
  margin-top: 20px;
}
</style>
