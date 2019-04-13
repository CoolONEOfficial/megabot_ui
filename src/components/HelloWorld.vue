<template>
    <v-container>
        <v-layout row>
            <v-flex xs8 class="LeftPart mr-5">
                <v-layout column>
                    <v-layout row class="align-center mb-3">
                        <img src="../assets/img/symbol-8-1.svg"
                             class="Logo">

                        <h1 class="LogoText ml-3" style="margin-top: 6px">МегаБот</h1>
                    </v-layout>

                    <v-layout row class="BigRect justify-space-around align-center mb-5 py-4">
                        <img :src="require(`@/assets/img/${name}.svg`)"
                             class="HeadIcon" v-for="name of ['symbol-7-2', 'symbol-6-2', 'symbol-4-2', 'symbol-5-2']"
                             :key="name">
                    </v-layout>

                    <v-layout row class="justify-space-between mb-5">
                        <v-layout column class="UsersItem align-center" v-for="(item, index) of userItems" :key="item">
                            <img :src="require(`@/assets/img/${item[1]}.svg`)"
                                 class="UsersItemSmile my-3">
                            <h4 class="UsersItemCaption">{{item[0]}}</h4>
                            <h1 class="UsersItemCount">{{requestCounts[index]}}</h1>
                        </v-layout>
                    </v-layout>

                    <v-layout row>
                        <v-flex class="align-center">
                            <v-layout row>
                                <h3 class="SubHeader mt-3 mr-3">Сводный отчет по организациям</h3>
                                <img src="../assets/img/symbol-18-1.svg"
                                     class="SubHeaderSearch my-3">
                            </v-layout>
                            <v-container style="max-height: 400px"
                                         class="scroll-y ma-0 pa-0">
                                <v-layout column>
                                    <v-layout row class="SmallRect pa-3 mb-3 align-center"
                                              v-for="item in organizations" :key="item">
                                        <img src="../assets/img/symbol-13-1.svg" class="OrgLogo mr-3">
                                        <v-layout column class="justify-center">
                                            <p class="OrgTitle mb-2">{{ item.name }}</p>
                                            <p class="GreyCaption mb-0">Лицевой счет: {{ item.number }}</p>
                                        </v-layout>
                                        <v-spacer></v-spacer>
                                        <img src="../assets/img/symbol-15-2.svg" class="OrgIcon">
                                        <img src="../assets/img/symbol-16-2.svg" class="OrgIcon mr-4">
                                    </v-layout>
                                </v-layout>
                            </v-container>
                        </v-flex>
                        <div style="width: 270px; margin-left: 140px;">
                            <h3 class="SubHeader mt-3">Логи ошибок</h3>
                        </div>
                    </v-layout>
                </v-layout>
            </v-flex>
            <v-flex xs4 class="RightPart ml-5">

            </v-flex>
        </v-layout>
    </v-container>
</template>

<script>
    class Organisation {
        constructor(
            name,
            number,
            users,
            icon,
        ) {
            this.name = name;
            this.number = number;
            this.icon = icon;
            this.users = users;
        }
    }

    class Error {
        constructor(
            number,
            text
        ) {
            this.number = number;
            this.text = text;
        }
    }

    export default {
        computed: {
            requestCounts() {
                return [
                    this.successCount,
                    this.failedCount,
                    this.successCount + this.failedCount,
                ];
            }
        },
        data() {
            return {
                successCount: 23,
                failedCount: 34,
                userItems: [
                    ['Обслуженных клиентов', 'symbol-3-2'],
                    ['Необслуженных запросов', 'symbol-2-2'],
                    ['Всего запросов', 'symbol-1-2'],
                ],
                errors: [
                    new Error(
                        234242, "Shit happens"
                    ), new Error(
                        12345, "Shit happens"
                    ), new Error(
                        24654, "Shit happens"
                    ), new Error(
                        65765, "Shit happens"
                    ), new Error(
                        546546, "Shit happens"
                    ), new Error(
                        234324, "Shit happens"
                    ),
                ],
                organizations: [
                    new Organisation(
                        "Сбер",
                        12312313123123,
                        23,
                        "icon.png"
                    ), new Organisation(
                        "tinkonfff",
                        43546546,
                        12
                    ), new Organisation(
                        "ваываыв",
                        4543645,
                        453
                    ),
                    new Organisation(
                        "Сбер",
                        12312313123123,
                        23,
                        "icon.png"
                    ), new Organisation(
                        "tinkonfff",
                        43546546,
                        12
                    ), new Organisation(
                        "ваываыв",
                        4543645,
                        453
                    ), new Organisation(
                        "Сбер",
                        12312313123123,
                        23,
                        "icon.png"
                    ), new Organisation(
                        "tinkonfff",
                        43546546,
                        12
                    ), new Organisation(
                        "ваываыв",
                        4543645,
                        453
                    ), new Organisation(
                        "Сбер",
                        12312313123123,
                        23,
                        "icon.png"
                    ), new Organisation(
                        "tinkonfff",
                        43546546,
                        12
                    ), new Organisation(
                        "ваываыв",
                        4543645,
                        453
                    ),
                    new Organisation(
                        "Сбер",
                        12312313123123,
                        23,
                        "icon.png"
                    ), new Organisation(
                        "tinkonfff",
                        43546546,
                        12
                    ), new Organisation(
                        "ваываыв",
                        4543645,
                        453
                    ), new Organisation(
                        "Сбер",
                        12312313123123,
                        23,
                        "icon.png"
                    ), new Organisation(
                        "tinkonfff",
                        43546546,
                        12
                    ), new Organisation(
                        "ваываыв",
                        4543645,
                        453
                    ),


                ],
            };
        }
    }
</script>

<style lang="scss">
    @import "colors.scss";
    @import "fonts.scss";

    .GreyCaption {
        opacity: 0.6;
        font-family: MagistralC;
        font-size: 14px;
        font-weight: normal;
        font-style: normal;
        font-stretch: normal;
        line-height: 1;
        letter-spacing: normal;
        color: $greyish-brown;
    }

    .OrgIcon {
        object-fit: contain;
        margin-left: 10px;
        cursor: pointer;
    }

    .OrgTitle {
        font-family: MagistralCond;
        font-size: 16px;
        font-weight: 300;
        font-style: normal;
        font-stretch: normal;
        line-height: 1;
        letter-spacing: 0.3px;
        text-align: left;
        color: $greyish-brown;
    }

    .OrgLogo {
        width: 52px;
        height: 52px;
    }

    .SubHeader {
        font-family: MagistralC;
        font-size: 20px;
        font-weight: normal;
        font-style: normal;
        font-stretch: normal;
        line-height: 1.1;
        letter-spacing: normal;
        text-align: left;
        color: $greyish-brown;
    }

    .UsersItemCount {
        font-family: Magistral;
        font-size: 58px;
        font-weight: bold;
        font-style: normal;
        font-stretch: normal;
        line-height: 1.33;
        letter-spacing: normal;
        text-align: center;
        color: #00b756;
    }

    .UsersItemCaption {
        font-family: Magistral;
        font-size: 16px;
        font-weight: bold;
        font-style: normal;
        font-stretch: normal;
        line-height: 1.31;
        letter-spacing: normal;
        text-align: center;
        color: #00b756;
    }

    .UsersItemSmile {
        width: 108.4px;
        height: 108.4px;
    }

    .UsersItem {
        max-width: 270px;
        border-radius: 28px;
        box-shadow: 0 0 4px 0 rgba(0, 0, 0, 0.05);
        background-color: $white;
    }

    .LeftPart {
        background: #f7f7f7;
    }

    .RightPart {
        box-shadow: -2px 0 6px 0 rgba(168, 168, 168, 0.09);
        background-color: $white;
    }

    .Logo {
        width: 59px;
        height: 17.2px;
        object-fit: contain;
    }

    .LogoText {
        font-family: Magistral;
        font-weight: lighter;
        font-size: 32px;
        font-style: normal;
        font-stretch: normal;
        line-height: 1.34;
        letter-spacing: 0.9px;
        text-align: left;
        color: $shamrock;
    }

    .BigRect {
        border-radius: 28px;
        box-shadow: 0 0 5px 0 rgba(0, 0, 0, 0.05);
        background-color: $white;
    }

    .SmallRect {
        border-radius: 18px;
        box-shadow: 0 0 4px 0 rgba(0, 0, 0, 0.05);
        background-color: $white;
    }

    .HeadIcon {
        width: 45px;
        height: 45px;
        object-fit: contain;
        cursor: pointer;
    }
</style>
