<template>
<div>
    <br>
    <br>
    <div  style="background:White">
          <h1  style="text-align:center; color:black"> Top 3 songs </h1>
         
        <br>
        <br>
        <BarChart
        :label="topSongs"
        :chart-data="topVotes" 
        
        ></BarChart>
        <br>
        <br>
         <PieChart
        :label="topSongs"
        :chart-data="topVotes" 
        
        ></PieChart>
        <br>
        <br>
        <!--here we bind the data the come from api calls, and we give to the properites we received from chart components  -->
        <PieChart
        :label="labels"
        :chart-data="votes" 
        
        ></PieChart>
        <br>
        <br>

        <BarChart
        :label="labels"
        :chart-data="votes" 
        
        ></BarChart>
        <br>
        <br>
      
    </div>
</div>
</template>

<script>
import Chart from 'chart.js/auto';
const axios=require("axios")

// importing charts
// thats 
import BarChart from './BarChart.vue'
import PieChart from './PieChart.vue'



export default {
    name: "Charts",
     data : ()=> {
        return {
            labels : [],
            songs : [],
            votes:[],
            topSongs:[],
            topVotes:[]
        }

    },
    // imporitng chart components where we will bind the data after making the property in the chart components 
    components : {    
  
        BarChart,
        PieChart
    },
    methods:{
      
     async  fetchSongs() {
       
      // Fetch all songs 
       const url = "http://webservies.be/eurosong/Songs";
      await fetch(url)
      // .then the output get output of fetch. comes as a promise 
        .then((response) => {
          //JSON parse the output
          return response.json();
        })
        .then((songs) => {
         this.songs = songs;
          this.fetchVotes(songs);
          this.fetchArtists(songs);
        });
    },


     fetchArtists(songs) {
     
      const url = "http://webservies.be/eurosong/Artists";
            fetch(url)
                .then((response) => response.json())
                .then((artists) => {
                // overwrite the existing array
                songs.map((song) => {
                    const artistId = song.artist; //3
                    // find the proper artist
                    const artist = artists.find(artist => artist.id == artistId);
                    // overwrite artist with the object
                    song.artist = artist;
                    //???
                    return song;
                });
                // overwrite the internal data
                this.songs = songs;
        });
    },
    fetchVotes(songs) {
      
      const url = "http://webservies.be/eurosong/Votes";
      fetch(url)
        .then((response) => response.json())
        .then((votes) => {
            // map brings an array with all elements it's like looping over the array

            songs.map((song) => {
                // intitlised new property inside our song object called total votes 
                song.totalVotes = 0

                // here we took the votes response. the we have foreach method whcich takes a function as a parameter 
              votes.forEach(myFunction);

              function myFunction(vote){
                  // the vote will be taken fsrom votes which indicated to each item in votes 
                  // if vote songID == song id then increment the total votes property 
                  if(vote.songID == song.id)
                  {
                      song.totalVotes += vote.points
                     
                  }
                
              }
              // this updated the data 
              return song;
            })
            // update our whole array 
            // and sort compare function. this functoin retruns negative, zero, or positive value
            // in our function we are comparing two songs total votes. and sort the value based on what the compare function returns 
            // if the retrun is negative we sort this value as lower
          
            this.songs =  songs.sort(function(a,b){return b.totalVotes - a.totalVotes});
        
            songs.forEach(song => {
             
              this.labels.push(song.title)
              this.votes.push(song.totalVotes)
            });
           for(var i = 0; i < 3; i++){
                  this.topSongs.push(songs[i].title)
                  this.topVotes.push(songs[i].totalVotes)
              }
          });

    
     
    },

  },
  // call the api once the page is loaded
   created(){
      this.fetchSongs();
  }
};
</script>