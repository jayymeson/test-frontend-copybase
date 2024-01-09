<!-- <template>
  <div>
    <churn-rate-chart
      v-if="chartData"
      :chart-data="chartData"
      :options="chartOptions"
    />
  </div>
</template>

<script>
import { Pie } from "vue-chartjs";
import { Chart as ChartJS, Tooltip, Title, ArcElement, Legend } from "chart.js";
import axios from "axios";

ChartJS.register(Tooltip, Title, ArcElement, Legend);

export default {
  extends: Pie,
  props: {
    month: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      chartData: null,
      cancellations: 0,
      chartOptions: {
        responsive: true,
        maintainAspectRatio: false,
        tooltips: {
          callbacks: {
            label: (tooltipItem, data) => {
              let label = data.labels[tooltipItem.index] || "";
              if (label === "Churn Rate") {
                label += `: ${Math.round(tooltipItem.yLabel * 100)}% (${
                  this.cancellations
                } cancellations)`;
              } else {
                label += `: ${Math.round(tooltipItem.yLabel * 100)}%`;
              }
              return label;
            },
          },
        },
      },
    };
  },
  mounted() {
    this.fetchData();
  },
  methods: {
    fetchData() {
      const apiUrl = `http://localhost:3000/metrics/churn-rate?start=${this.month}`;
      axios
        .get(apiUrl)
        .then((response) => {
          const { churnRate, cancellations } = response.data;
          this.chartData = {
            labels: ["Churn Rate", "Retained"],
            datasets: [
              {
                data: [churnRate * 100, 100 - churnRate * 100],
                backgroundColor: ["#FF6384", "#36A2EB"],
                hoverBackgroundColor: ["#FF6384", "#36A2EB"],
              },
            ],
          };
          this.cancellations = cancellations;
        })
        .catch((error) => {
          console.error("Error fetching data:", error);
        });
    },
  },
  watch: {
    month(newVal, oldVal) {
      if (newVal !== oldVal) {
        this.fetchData();
      }
    },
  },
};
</script>

<style scoped>
/* Estilos do componente */
</style> -->
