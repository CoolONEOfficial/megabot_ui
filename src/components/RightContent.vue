<template>
    <div style="height: 100vh">
        <v-text-field
                height="76px"
                v-if="searchHistoryShow"
                v-model="searchHistoryModel"
                label="Поиск"
                solo
                style="position: absolute; right: 76px; left: 66.6%;"
        ></v-text-field>
        <v-layout row>
            <v-spacer></v-spacer>
            <div class="SearchIcon" @click="searchHistoryShow = !searchHistoryShow"
                 :style="`background-color: ${searchHistoryShow ? 'limegreen' : '#00b856'};`">
                <img src="../assets/img/symbol-10-3.svg" class="align-center"
                     style="width: 32px; height: 32px; margin: 22px;"/>
            </div>
        </v-layout>
        <v-layout row class="align-center pl-4">
            <img src="../assets/img/symbol-11-3.svg" class="align-center"
                 style="width: 32px; height: 32px; margin: 22px;"/>
            <h1 class="SectionTitle mt-2">Последние запросы</h1>
            <v-spacer></v-spacer>
            <img src="../assets/img/symbol-9-3.svg" class="align-center"
                 style="width: 32px; height: 32px; margin: 22px;"/>
        </v-layout>

        <vue-custom-scrollbar style="height: 80vh" class="pl-4">
            <v-layout column v-for="[sectionDate, section] in Object.entries(history)" :key="sectionDate" class="pr-4">
                <v-layout column>
                    <v-layout row class="mt-5 align-center">
                        <div class="Circle ml-4 mr-3"></div>
                        <h1 class="SectionTitle">{{ sectionDate }}</h1>
                    </v-layout>
                    <v-divider class="mb-1"></v-divider>
                </v-layout>
                <v-layout v-for="(mRequest, reqIndex) in searchHistoryShow ? section.filter(
                          request => request.companyname.toLocaleLowerCase().includes(searchHistoryModel.toLocaleLowerCase())
                                  || request.action.toLocaleLowerCase().includes(searchHistoryModel.toLocaleLowerCase())
                          ) : section" :key="reqIndex">
                    <v-layout row class="align-center mt-2">
                        <p class="RequestTime ma-0">{{ mRequest.time }}</p>
                        <p class="RequestCompany ma-0" style="position: absolute; left: 17%;">
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
    import axios from "axios";

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
        async mounted() {
            console.log("startt");
            let data = (await axios
                .get('http://cometbot.ru/scripts/getdb.php', {
                        params: {
                            query: 'SELECT * FROM requests',
                        }
                    }
                )).data;

            for (let requestId in data) {
                let request = data[requestId];
                console.log(request);
                let timestamp = parseInt(request.datetime);
                console.log('timestamp: ', timestamp);
                let date = new Date(timestamp * 1000);
                let dateMonth = this.monthes[date.getMonth()] + ', ' + date.getDate();

                if (!(dateMonth in this.history)) {
                    this.$set(this.history, dateMonth, []);
                }

                this.history[dateMonth].push(new Request(
                    date.toLocaleTimeString(),
                    request.companyname,
                    request.action,
                ));
            }
        },
        data() {
            return {
                monthes: [
                    'Январь',
                    'Февраль',
                    'Март',
                    'Апрель',
                    'Май',
                    'Июнь',
                    'Июль',
                    'Август',
                    'Сентябрь',
                    'Октябрь',
                    'Ноябрь',
                    'Декабрь',
                ],
                history: {},
                searchHistoryModel: '',
                searchHistoryShow: false,
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
        cursor: pointer;
    }

     label {
        font-size: 30pt;
    }
</style>
