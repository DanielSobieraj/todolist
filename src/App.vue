<template>
    <v-app>
        <v-container
                fill-height>
            <v-row justify="center">
                <v-col
                        cols="6">
                    <ul>
                        <li :key="todo.index" v-for="todo in newTodo">
                            {{ todo.names }} Index: {{ todo.index }}
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
    const STORAGE_KEY = "Vue-todo";
    let todoStorage = {
        fetch() {
            let todos = JSON.parse(localStorage.getItem(STORAGE_KEY));
            todos.forEach(function (todo, index) {
                todo.id = index;
            });
            todoStorage.uid = todos.length;
            return todos
        },
        save(todos) {
            localStorage.setItem(STORAGE_KEY, JSON.stringify(todos));
        }
    };

    export default {
        name: 'App',
        data() {
            return {
                todos: todoStorage.fetch(),
                todo: '',
                newTodo: [],
            };
        },
        methods: {
            addTodo() {
                this.newTodo.push({
                    "names": this.todo,
                });
                localStorage.setItem("names", JSON.stringify(this.newTodo));
                this.todo = '';
            },
            removeTodo(todo) {
                this.newTodo.splice(this.newTodo.indexOf(todo), 1)
            }
        },
        created() {
            localStorage.getItem("names")
        }
    };
</script>
