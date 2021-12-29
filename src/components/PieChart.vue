<template>
  <div>
      <h1>Pie chart</h1>
    <canvas ref="pieChart"></canvas>
  </div>
</template>

<script>
import Chart from 'chart.js/auto';

// importing data from planet chart data 



export default {
 props:{
   label:{
     type: Object
   },
   chartData:{
     type: Array
   }
 },
   async mounted() {
    await new Chart(this.$refs.pieChart, {
      type: 'pie',
      data: {
        labels: this.label,
        datasets: [
        {
          axis:'y',
            label: 'Songs labels',
            backgroundColor: 'rgba(144,238,144 , 0.9 )',
            data: this.chartData,
            fill: false,
    backgroundColor: [
      'rgba(255, 99, 132, 0.2)',
      'rgba(255, 159, 64, 0.2)',
      'rgba(255, 205, 86, 0.2)',
      'rgba(75, 192, 192, 0.2)',
      'rgba(54, 162, 235, 0.2)',
      'rgba(153, 102, 255, 0.2)',
      'rgba(201, 203, 207, 0.2)'
    ],
    borderColor: [
      'rgb(255, 99, 132)',
      'rgb(255, 159, 64)',
      'rgb(255, 205, 86)',
      'rgb(75, 192, 192)',
      'rgb(54, 162, 235)',
      'rgb(153, 102, 255)',
      'rgb(201, 203, 207)'
    ],

        }
        ]
      },
    options: {
    responsive: true,
    plugins: {
      legend: {
        position: 'top',
      },
      title: {
        display: true,
        text: ''
      }
    }
  },

    });
  }
}

</script>