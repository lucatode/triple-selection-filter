<template>
    <v-app>
        <v-content>
            <v-container>
                <v-layout row>
                    <v-autocomplete
                            :items="getItems.map(x=>x.language)"
                            v-model="language"
                    ></v-autocomplete>
                    <v-autocomplete
                            :items="getItems
                                      .filter(x=>x['language']===language)
                                      .map(x=>x.file)"
                            v-model="file"
                    ></v-autocomplete>
                    <v-autocomplete
                            :items="getItems
                                      .filter(x=>x['language']===language)
                                      .filter(x=>x['file']===file)
                                      .map(x=>x.version)"
                            v-model="version"
                    ></v-autocomplete>
                </v-layout>
                <v-layout row>
                    {{getText(language, file, version)}}
                </v-layout>
            </v-container>
        </v-content>
    </v-app>
</template>

<script>

    import {mapActions, mapGetters} from "vuex";

    export default {
        name: 'App',

        components: {},

        data: () => ({
            //
            language: {},
            file: {},
            version: {}
        }),

        computed: {
            ...mapGetters(['getItems'])
        },

        methods: {
            ...mapActions(['loadItems']),
            getText(language, file, version) {
                let item = this.getItems
                    .filter(x => x['language'] === language)
                    .filter(x => x['file'] === file)
                    .filter(x => x['version'] === version);
                return item.length === 1 ? item[0].text : ''
            }
        },

        created() {
            this.loadItems();
        }

    };
</script>
