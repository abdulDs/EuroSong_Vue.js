<template>
    <nav class="c-nav">
        <h2 v-html="title" v-on:click="toggleNav">
        </h2>

        <ul class="c-nav-list" v-if="showNavigation">
            <li class="c-nav-list-item" v-for="(item, index) in items" :key="index">
                <a :href="item.url" :disabled="item.disabled" :id="item.id" :target="item.target">
                    {{ item.text }}
                </a>
            </li>
        </ul>
    </nav>
</template>

<script>
    export default {
        name: "Navigation",
        data()  {
            return {
                showNavigation: false,
                title: "Home <strong>page</strong>",
                items: [
                    {
                        text: "Home",
                        url: "https://google.be/home",
                        disabled: true,
                        id: "home-navigation",
                        target: "_self"
                    },
                    {
                        text: "Voting",
                        url: "https://google.be/voting",
                        disabled: false,
                        id: "vote-navigation",
                        target: "_blank"
                    },
                    {
                        text: "Contact",
                        url: "https://google.be/contact",
                        disabled: false,
                        id: "contact-navigation",
                        target: "_self"
                    },
                ]
            }
        },
        methods: {
            toggleNav() {
                this.showNavigation = !this.showNavigation;
            }
        }
    }
</script>