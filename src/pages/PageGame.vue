<template>
    <div>
        <button @click="changePage('home')">
        Go back to home
        </button>

        <div v-if="!endOfGame">
            <Carousel 
            :songs="songs"
            :activeSongIndex="activeSongIndex"

            @change-song-index="changeActiveSongIndex"
            />

            <div v-for="(vote, index) in votes" :key="index">
                <button v-if="!vote.isVoted" @click="sendVote(vote.points)">
                    Add {{vote.points}}
                </button>
            </div>
        </div>

        <div v-if="endOfGame">
            You voted everything
        </div>
    </div>
</template>

<script>
    // components
    import Carousel from "../components/Carousel.vue";

    // settings of component itself
    export default {
    name: "PageGame",
    components: {
        Carousel
    },
    data() {
        return {
            songs: [],
            activeSongIndex: 0,
            endOfGame: false,
            votes: [
                {
                    points: 1,
                    isVoted: false
                },
                {
                    points: 2,
                    isVoted: false
                },
                {
                    points: 4,
                    isVoted: false
                },
                {
                    points: 8,
                    isVoted: false
                },
            ]
        };
    },
    mounted() {
        this.fetchSongs();
    },
    methods: {
        // change internal data
        changePage(page) {
            this.$emit("change-page", page);
        },

        changeActiveSongIndex(index) {
            this.activeSongIndex = index;
        },

        sendVote(points) {
            // hidding the vote button
            this.votes.map((vote) => {
                if (vote.points == points) {
                    vote.isVoted = true;
                }
            })

            // post vote to the api
            const songID = this.songs[this.activeSongIndex].id;

            fetch("http://webservies.be/eurosong/Votes", {
                method: "POST",
                headers: {
                    'Accept': 'application/json, text/plain',
                    'Content-Type': 'application/json;charset=UTF-8'
                },
                body: JSON.stringify({
                    songID: songID,
                    points: points
                })
            }).then((response) => {
                return response.json()
            }).then((answer) => {
                console.log(answer);
            });

            // check the game is done
            const activeVotes = this.votes.filter((vote) => !vote.isVoted);

            if (activeVotes.length == 0) {
                this.endOfGame = true;
            }

            // es5 way
            // let endOfGame = true;

            // for (let i = 0; i < this.votes.length; i++) {
            //     if (this.votes[i].isVoted == false) {
            //         endOfGame = false;
            //     }
            // }

            // if (endOfGame) {
            //     alert("game is done");
            // }
        },

        // fetch
        fetchSongs() {
            const url = "http://webservies.be/eurosong/Songs";
            fetch(url)
                .then((response) => response.json())
                .then((songs) => {
                this.mapArtistOnSongs(songs);
            });
        },
        mapArtistOnSongs(songs) {
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
                    return song;
                });
                // overwrite the internal data
                this.songs = songs;
            });
        }
    },
    components: { Carousel }
}
</script>