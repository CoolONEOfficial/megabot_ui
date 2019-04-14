<template>
    <v-layout column>
        <v-layout row class="align-center mb-2">
            <img src="../assets/img/symbol-8-1.svg"
                 class="Logo">

            <h1 class="LogoText ml-3" style="margin-top: 6px">МегаБот</h1>
        </v-layout>

        <v-layout  row class="BigRect justify-space-around align-center mb-3 py-4" :style="`opacity: ${auth == null ? 0.5 : 1};`">
            <img :src="require(`@/assets/img/1_${auth ? 'on' : 'off'}.svg`)"
                 class="HeadIcon" @click="auth = !auth" alt="Авторизация">

            <img :src="require(`@/assets/img/2_${office ? 'on' : 'off'}.svg`)"
                 class="HeadIcon" @click="office = !office" alt="Личный кабинет">

            <img :src="require(`@/assets/img/3_${service ? 'on' : 'off'}.svg`)"
                 class="HeadIcon" @click="service = !service" alt="Дистанционное обслуживание">

            <img :src="require(`@/assets/img/4_${tariff ? 'on' : 'off'}.svg`)"
                 class="HeadIcon" @click="tariff = !tariff" alt="Тарифы и услуги">
        </v-layout>

        <v-layout row class="justify-space-between mb-1">
            <v-layout column class="UsersItem align-center" v-for="(item, index) of userItems" :key="item">
                <img :src="require(`@/assets/img/${item[1]}.svg`)"
                     class="UsersItemSmile my-3">
                <h4 class="UsersItemCaption">{{item[0]}}</h4>

                <h1 class="UsersItemCount" v-if="requestCounts[index] !== 0">{{requestCounts[index]}}</h1>
                <v-progress-circular indeterminate class="ma-3" v-else></v-progress-circular>
            </v-layout>
        </v-layout>

        <v-layout row>
            <div class="align-center" style="width: 63%;">
                <v-layout row>
                    <h3 class="SubHeader mt-3 mr-3">Сводный отчет по организациям</h3>
                    <img src="../assets/img/symbol-18-1.svg"
                         class="SubHeaderSearch my-3">
                </v-layout>
                <vue-custom-scrollbar style="max-height: 400px">
                    <v-layout column>
                        <v-layout row class="SmallRect py-3 pl-3 mr-3 mb-3 align-center"
                                  v-for="item in organizations" :key="item">
                            <img :src="require(`@/assets/img/${item.icon}.png`)" class="OrgLogo mr-3">
                            <v-layout column class="justify-center">
                                <p class="OrgTitle mb-2">{{ item.name }}</p>
                                <p class="GreyCaption mb-0">Лицевой счет: {{ item.orgId }}</p>
                            </v-layout>
                            <v-spacer></v-spacer>
                            <img src="../assets/img/symbol-15-2.svg" class="OrgIcon">
                            <img src="../assets/img/symbol-16-2.svg" class="OrgIcon mr-4">
                        </v-layout>
                    </v-layout>
                </vue-custom-scrollbar>
            </div>
            <v-spacer></v-spacer>
            <div style="width: 23%;">
                <h3 class="SubHeader mt-3 mb-3">Логи ошибок</h3>
                <v-layout column class="SmallRect py-3 pl-3 pr-2">
                    <vue-custom-scrollbar style="max-height: 200px">
                        <p class="ErrCaption" v-for="item in errors" :key="item">
                            #{{item.orgId}}: {{item.text}}
                        </p>
                    </vue-custom-scrollbar>
                </v-layout>
                <h3 class="SubHeader mt-3 mb-3">Форма ответного письма</h3>
                <v-layout column class="SmallRect px-3 py-2 justify-center">
                    <div v-for="item in buttons" :key="item">
                        <v-layout row>
                            <p class="ErrCaption px-1 py-2 ma-0">
                                {{item}}
                            </p>
                            <v-spacer></v-spacer>
                            <img src="../assets/img/symbol-19-1.svg"
                                 class="Symbol-19-1">
                        </v-layout>
                        <v-divider v-if="item !== buttons[buttons.length - 1]"></v-divider>
                    </div>
                </v-layout>
            </div>
        </v-layout>
    </v-layout>
</template>

<script>
    import vueCustomScrollbar from 'vue-custom-scrollbar'
    import axios from "axios";
    import http from 'http';

    class Organisation {
        constructor(
            name,
            orgId,
            users,
            icon,
        ) {
            this.name = name;
            this.orgId = orgId;
            this.icon = icon;
            this.users = users;
        }
    }

    class Error {
        constructor(
            accountId,
            text
        ) {
            this.accountId = accountId;
            this.text = text;
        }
    }

    export default {
        components: {
            vueCustomScrollbar
        },
        methods: {
            get_json(url, callback) {
        http.get(url, function(res) {
            var body = '';
            res.on('data', function(chunk) {
                body += chunk;
            });

            res.on('end', function() {
                var response = JSON.parse(body);
// call function ----v
                callback(response);
            });
        });
    },
            saveIni(key, value) {
                axios.get(
                    'http://cometbot.ru/scripts/prefs_writer.php', {
                        params: {
                            key: key,
                            value: value,
                        }
                    }
                );
            }
        },
        async mounted() {
            let data = (await axios
                .get('http://cometbot.ru/scripts/getdb.php', {
                        params: {
                            query: 'SELECT * FROM organisations',
                        }
                    }
                )).data;

            for (let orgId in data) {
                let org = data[orgId];
                this.organizations.push(
                    new Organisation(
                        org.title,
                        org.accountId,
                        3,
                        org.icon
                    )
                );
            }

            data = (await axios
                .get('http://cometbot.ru/scripts/getdb.php', {
                        params: {
                            query: 'SELECT * FROM parsed WHERE status=0',
                        }
                    }
                )).data;

            this.successCount = data.length;

            data = (await axios
                .get('http://cometbot.ru/scripts/getdb.php', {
                        params: {
                            query: 'SELECT * FROM parsed WHERE status=1',
                        }
                    }
                )).data;

            this.failedCount = data.length;

            data = (await axios
                .get('http://cometbot.ru/scripts/prefs_reader.php', {})).data;

            console.log('dadaata:', data);

            this.auth = data.auth === "true";
            this.service = data.service === "true";
            this.tariff = data.tariff === "true";
            this.office = data.office === "true";
        },
        watch: {
            auth(nauth) {
                this.saveIni('auth', nauth);
            },
            office(noffice) {
                this.saveIni('office', noffice);
            },
            service(nservice) {
                this.saveIni('service', nservice);
            },
            tariff(ntariff) {
                this.saveIni('tariff', ntariff);
            },
        },
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
                auth: null,
                office: null,
                service: null,
                tariff: null,
                successCount: 0,
                failedCount: 0,
                userItems: [
                    ['Обслуженных клиентов', 'symbol-3-2'],
                    ['Необслуженных запросов', 'symbol-2-2'],
                    ['Всего запросов', 'symbol-1-2'],
                ],
                buttons: [
                    'Авторизация',
                    'Подключение\nличного кабинета',
                    'Смена тарифа',
                    'Дист. обслуживание',
                    'Включение / выключение услуг',
                ],
                errors: new Array(20).fill(new Error(
                    234242, "Shit happens"
                )),
                organizations: [],
            };
        }
    }
</script>

<style lang="scss">
    @import "colors.scss";
    @import "fonts.scss";

    .ErrCaption {
        opacity: 0.6;
        font-family: MagistralCond, serif;
        font-size: 16px;
        font-weight: 300;
        font-style: normal;
        font-stretch: normal;
        line-height: 0.9;
        letter-spacing: 0.5px;
        text-align: left;
        color: $greyish-brown;
    }

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
        font-weight: normal;
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
        font-weight: normal;
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
