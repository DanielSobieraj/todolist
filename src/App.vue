<template>
    <v-app>
        <v-container
                fill-height>
            <v-row justify="center">
                <v-col
                        cols="6">
                    <ul>
                        <li :key="todo.index" v-for="todo in newTodo">
                            {{ todo.name }} {{ todo.date }}
                            <v-btn
                                    @click="removeTodo"
                                    class="destroy">X
                            </v-btn>
                        </li>
                    </ul>
                </v-col>
            </v-row>
            <v-row justify="center">
                <v-col
                        cols="6">
                    <v-text-field
                            autofocus
                            autocomplete="off"
                            placeholder="What need to be done?"
                            v-model="todo"
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
                todo: '',
                newTodo: [],
            };
        },
        methods: {
            addTodo() {
                this.newTodo.push({
                    "index": 1,
                    "name": this.todo,
                });
                this.todo = ''
            },
            removeTodo(todo) {
                this.newTodo.splice(this.newTodo.indexOf(todo),1)
            }
        }
    };
</script>
