<template>
  <div>
      <h1>Hello</h1>
    <canvas ref="c"></canvas>
  </div>
</template>

<script>
import Chart from 'chart.js/auto';

// importing data from planet chart data 



export default {
 props:{
   label:{
     type: Array
   },
   chartData:{
     type: Array
   }
 },
   async mounted() {
    await new Chart(this.$refs.c, {
      type: 'bar',
      data: {
        labels: this.label,
        datasets: [
        {
            label: 'this',
            backgroundColor: 'rgba(144,238,144 , 0.9 )',
            data: this.chartData,
        }
        ]
      },
    //   options: {
    // scales: {
    //   yAxes: [{
    //     ticks: {
    //       beginAtZero: false
    //     }
    //   }]
    // }
    //}
    });
  }
}

</script>