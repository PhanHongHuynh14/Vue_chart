<template>
  <div class="chart-container">
    <h4>Free Members This Year</h4>
    <div class="card-body">
      <canvas id="xyChartId"></canvas>
    </div>
  </div>
</template>

<script>
import Chart from 'chart.js/auto';
import 'chartjs-plugin-datalabels';
import axios from 'axios';

export default {
  props: {
    chartTitle: String,
    xyChartId: String
  },
  data() {
    return {
      employeeData: []
    };
  },
  mounted() {
    this.fetchData();
  },

  methods: {
    async fetchData() {
      await axios.get('http://localhost:8000/api/worker', { withCredentials: true })
        .then(res => {
          this.employeeData = res.data.data;
          this.createChart();
        }).catch(error => {
          console.error('Error fetching data from API:', error);
        });
    },

    createChart() {
      const uniqueSections = [...new Set(this.employeeData.map(employee => employee.section_name))];
      const sectionData = {};

      uniqueSections.forEach(section => {
        const filteredData = this.employeeData.filter(employee => employee.section_name === section);
        sectionData[section] = filteredData.length;
      });

      const labels = Object.keys(sectionData);
      const data = Object.values(sectionData);

      const chartData = {
        labels: labels,
        datasets: [
          {
            label: 'Sections',
            data: data,
            backgroundColor: ['#5eb0d5', '#5c8bd5', '#5c69d3', '#7360d4', '#9960d3', '#d65fc4', '#d75e9f', '#66CCFF', '#d65c7c', '#669999', '#d65e5e', '#0033cc', '#d65e5e'],
            borderColor: '#fff',
            borderWidth: 2.5
          }
        ]
      };

      const options = {
        responsive: true,
        maintainAspectRatio: false,
      };

      const ctx = document.getElementById('xyChartId');
      if (ctx) {
        const ctxContext = ctx.getContext("2d");
        new Chart(ctx, {
          type: 'bar',
          data: chartData,
          options: options
        });
      }
    },
  }
};
</script>
