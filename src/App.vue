<template>
  <div id="app">

<!-- -->
    <!-- Page home -->
    <PageHome
      v-if="page == 'home'"
    
      @change-page="changePage"
    />

    <!-- Page game -->
    <PageGame
      v-if="page == 'game'"

      @change-page="changePage"
    />

    <!-- Page ranking -->
    <PageRanking
      v-if="page == 'ranking'"

      @change-page="changePage"
    />
  </div>
</template>

<script>
  // Components
  import PageHome from "./pages/PageHome.vue";
  import PageRanking from "./pages/PageRanking.vue";
  import PageGame from "./pages/PageGame.vue";
  import PlanetChart from "./components/PlanetChart.vue";

  // Scss
  import style from "./scss/style.scss";

  // Export
  export default {
    name: 'App',
    data() {
      return {
        page: "game"
      }
    },
    components: {
      PageHome,
      PageRanking,
      PageGame,
      PlanetChart
    },
    methods: {
      changePage(page) {
        this.page = page;
      }
    }
  }
</script>