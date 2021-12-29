<template>
<div>
    <br>
    <br>
    <div>

        <br>
        <!--here we bind the data the come from api calls, and we give to the properites we received from chart components  -->
        
        <br>
        <br>

        <PlanetChart
        :label="labels"
        :chart-data="votes" 
        
        ></PlanetChart>
    </div>
</div>
</template>

<script>
import Chart from 'chart.js/auto';
const axios=require("axios")

// importing charts
// thats 
import PlanetChart from './PlanetChart.vue'



export default {
    name: "Songs",
     data : ()=> {
        return {
            labels : [],
            songs : [],
            votes:[]
        }

    },
    // imporitng chart components where we will bind the data after making the property in the chart components 
    components : {    
  
        PlanetChart
    },
    async created() {
  const { data } = await axios.get("http://webservies.be/eurosong/Songs");
 

  var c=0
  for(var i=0;i<data.length;i++){
          data[i].totalVotes = 0
            if (data[i].title in this.labels){
              continue
            }
            else{

              this.labels.push(data[i].title)

              // I need to find
              // votesData.forEach(element => {
              //    if (element.songID == data[i].id ) {
              //   data[i].totalVotes += element.votes
              // }
              // });
             //this.votes.push(votesData[i].points)

              c=c+1
              if(c==data.length){
                break
              }
            }
               const { votesData } = await axios.get("http://webservies.be/eurosong/Votes");
            votesData.forEach(element => {
              console.log(element);
            });
          
  }
console.log(this.votes)
}   
}
</script>