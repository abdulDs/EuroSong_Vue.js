<template>
    <div>
      <h1>
        Eurosong
      </h1>

      <button @click="changePage('game')">
        Show game
      </button>
      <br>
      <button @click="changePage('ranking')">
        Show ranking
      </button>
    </div>
</template>

<script>
    export default {
        name: "PageHome",
        methods: {
            changePage(page) {
                this.$emit("change-page", page);
            }
        }
    }
</script>