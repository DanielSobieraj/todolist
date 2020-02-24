<template>
    <v-app>
        <v-container
                fill-height>
            <v-row justify="center">
                <v-col
                        cols="6">
                </v-col>
            </v-row>
            <v-row justify="center">
                <v-col
                        cols="6">
                    <v-text-field
                            autofocus
                            autocomplete="off"
                            placeholder="What need to be done?"
                            v-model="newTodo"
                            @keyup.enter="addTodo">
                    </v-text-field>
                    <v-btn
                            @click="addTodo">
                        Add
                    </v-btn>
                </v-col>
            </v-row>
        </v-container>
    </v-app>
</template>

<script>

    export default {
        name: 'App',
        data() {
            return {
                newTodo: [],
            };
        },
        methods: {
            addTodo() {
                this.newTodo.push({value: ''});
            }
        }
    };
</script>
