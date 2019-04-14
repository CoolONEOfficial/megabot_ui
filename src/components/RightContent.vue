<template>
    <div style="height: 100vh" class="pl-4">
        <v-layout row>
            <v-spacer></v-spacer>
            <div class="SearchIcon">
                <img src="../assets/img/symbol-10-3.svg" class="align-center"
                     style="width: 32px; height: 32px; margin: 22px;"/>
            </div>
        </v-layout>
        <v-layout row class="align-center">
            <img src="../assets/img/symbol-11-3.svg" class="align-center"
                 style="width: 32px; height: 32px; margin: 22px;"/>
            <h1 class="SectionTitle mt-2">Последние запросы</h1>
            <v-spacer></v-spacer>
            <img src="../assets/img/symbol-9-3.svg" class="align-center"
                 style="width: 32px; height: 32px; margin: 22px;"/>
        </v-layout>

        <vue-custom-scrollbar style="height: 80vh">
            <v-layout column v-for="[sectionDate, section] in Object.entries(history)" :key="sectionDate" class="pr-4">
                <v-layout column>
                    <v-layout row class="mt-5 align-center">
                        <div class="Circle ml-4 mr-3"></div>
                        <h1 class="SectionTitle">{{ sectionDate }}</h1>
                    </v-layout>
                    <v-divider class="mb-1"></v-divider>
                </v-layout>
                <v-layout v-for="(mRequest, reqIndex) in section" :key="reqIndex">
                    <v-layout row class="align-center mt-2">
                        <p class="RequestTime ma-0">{{ mRequest.time }}</p>
                        <p class="RequestCompany ma-0" style="position: relative; left: 7%;">
                            {{ mRequest.companyname }}
                        </p>
                        <v-spacer></v-spacer>
                        <p class="RequestAction ma-0 mr-3">{{ mRequest.action }}</p>
                        <img src="../assets/img/symbol-13-3.svg">
                    </v-layout>
                </v-layout>
            </v-layout>
        </vue-custom-scrollbar>
    </div>
</template>

<script>
    import vueCustomScrollbar from 'vue-custom-scrollbar'

    class Request {
        constructor(
            time,
            companyname,
            action
        ) {
            this.time = time;
            this.companyname = companyname;
            this.action = action;
        }

    }

    export default {
        components: {
            vueCustomScrollbar,
        },
        name: "RightContent",
        data() {
            return {
                history: {
                    "Апрель, 12": new Array(10).fill(new Request(
                        "12:32:43",
                        "Сбер",
                        "ЛКККК"
                    )),
                    "Апрель, 11": new Array(10).fill(new Request(
                        "12:32:43",
                        "Сбер",
                        "ЛКККК"
                    )),
                    "Апрель, 10": new Array(10).fill(new Request(
                        "12:32:43",
                        "Сбер",
                        "ЛКККК"
                    ))
                },
            };
        },
    }
</script>

<style scoped lang="scss">
    @import "colors";
    @import "fonts";

    .RequestTime {
        font-family: MagistralCond;
        font-size: 20px;
        font-weight: 300;
        font-style: normal;
        font-stretch: normal;
        line-height: 1.2;
        letter-spacing: normal;
        text-align: left;
        color: $greyish-brown;
    }

    .RequestCompany {
        @include Text-Style;
    }

    .RequestAction {
        font-family: MagistralCond;
        font-size: 20px;
        font-weight: 300;
        font-style: normal;
        font-stretch: normal;
        line-height: 1.2;
        letter-spacing: normal;
        text-align: right;
        color: $greyish-brown;
    }

    .SectionTitle {
        @include Text-Style-2;
        font-weight: lighter;
    }

    .SearchIcon {
        width: 76px;
        height: 76px;
        background-color: $shamrock;
        cursor: pointer;
    }
</style>
