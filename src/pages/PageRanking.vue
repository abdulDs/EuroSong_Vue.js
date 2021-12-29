<template>
  <div>
    <button @click="changePage('home')">Go back to home</button>
    <br />
<!-- this where we have all our charts -->
    <Charts/>
 

  <table>
        <tr>
          <!-- Table headers -->
          <th>Ranking for</th>
          <th>Title</th>
          <th>Artist</th>
          <th>Points</th>
          <th>Spotify Refference</th>
        </tr>
        <!-- Looping through our songss using v-for(for each song in songs, key is the index here) -->
        <tr v-for="(song, i) in songs" :key="i">
          <!-- Ranking number -->
          <td>#{{ i + 1 }}</td>
          <!-- Song details -->
          <td>{{ song.title }}</td>
          <td>{{ song.artist.name }}</td>
          <td>{{ song.totalVotes }}</td>
          <!-- Fetch spotify song -->
          <td>
            <iframe
              :src="song.spotify"
              height="80"
              width="100%"
              frameBorder="0"
            ></iframe>
          </td>
        </tr>
      </table>
    <br>
  </div>

</template>

<script>
// import components

import Charts from '../components/Charts.vue';
import BarChart from '../components/BarChart.vue';
import PieChart from '../components/PieChart.vue';




export default {
  name: "PageRanking",
  data() {
    return {
        songs : []
    };
  },
  components:{
    
    Charts,
    BarChart,
    PieChart

 
  },
  methods: {
    changePage(page) {
      this.$emit("change-page", page);
    },
        fetchSongs() {
      // Fetch all songs 
      const url = "http://webservies.be/eurosong/Songs";
      fetch(url)
      // .then the output get output of fetch. comes as a promise 
        .then((response) => {
          //JSON parse the output
          return response.json();
        })
        .then((x) => {
         this.songs = x;
          this.fetchVotes(x);
          this.fetchArtists(x);
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
          });

    },

  },
  // call the api once the page is loaded
  mounted(){
      this.fetchSongs();
  }
};
</script>
