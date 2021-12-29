<template>
    <button @click="clicked">
        {{buttonText}}
    </button>
</template>

<script>
    export default {
        name: "Button",
        props: [
            "alertMessage",
            "alertType",
            "buttonText"
        ],
        methods: {
            clicked() {
                this.$emit("handle-clicked", this.alertMessage, this.alertType);
            }
        }
    }
</script>