<template>
    <v-app id="light">
        <v-content>
            <v-layout row>
                <v-flex xs8>
                    <LeftContent class="px-5 pt-3"/>
                </v-flex>
                <v-flex xs4>
                    <RightContent id="right-panel"></RightContent>
                </v-flex>
            </v-layout>
            <v-snackbar
                    v-model="snackbar"
                    bottom right
            >
                {{ snacktext }}
            </v-snackbar>
        </v-content>
    </v-app>
</template>

<script>
    import LeftContent from './components/LeftContent'
    import RightContent from "./components/RightContent";
    import axios from 'axios';

    export default {
        name: 'App',
        components: {
            RightContent,
            LeftContent,
        },
        data() {
            return {
                snackbar: false,
                snacktext: "sdfjkdlsfsdlf",
            }
        }
    }
</script>

<style lang="scss">
    @import "components/colors.scss";

    #light {
        background: #f7f7f7;
    }

    #right-panel {
        box-shadow: -2px 0 6px 0 rgba(168, 168, 168, 0.09);
        background-color: $white;
    }

    .Circle {
        background: $shamrock;
        width: 6px;
        height: 6px;
        border-radius: 50%;
    }
</style>
