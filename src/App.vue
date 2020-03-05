<template>
    <v-app>
        <v-container
                fill-height>
            <v-row justify="center">
                <v-col
                        cols="10"
                        md="6">
                    <v-text-field
                            @keyup.enter="addTodo"
                            autocomplete="off"
                            autofocus
                            clearable
                            counter
                            outlined
                            placeholder="What need to be done?"
                            v-model="newTodo">
                    </v-text-field>
                </v-col>
            </v-row>
            <v-row justify="center">
                <v-col
                        cols="10"
                        md="6"
                        style="border: 1px solid grey">
                    <div :key="todo.id"
                         class="d-flex justify-space-between align-center"
                         v-for="(todo, index) in todosFiltered">
                        <div class="d-flex align-center">
                            <v-checkbox color="success" v-model="todo.completed"></v-checkbox>
                            <v-list>
                                <p
                                        :class="{ completed : todo.completed }"
                                        @dblclick="editTodo(todo)"
                                        class="mb-0"
                                        v-if="!todo.editing">
                                    {{ todo.title }}
                                </p>
                                <v-text-field
                                        @blur="doneEdit(todo)"
                                        @keyup.enter="doneEdit(todo)"
                                        @keyup.esc="cancelEdit(todo)"
                                        autofocus
                                        v-else
                                        v-model="todo.title"
                                >

                                </v-text-field>
                            </v-list>
                        </div>
                        <div class="">
                            <v-btn
                                    @click="removeTodo(index)"
                                    class="destroy"
                                    color="red"
                                    dark
                                    rounded
                                    x-small> X
                            </v-btn>
                        </div>
                    </div>
                    <hr>
                    <div class="d-flex align-center justify-space-between">
                        <div class="d-flex align-center">
                            <v-checkbox
                                    :value="!anyRemaining"
                                    @change="checkAllTodos"
                                    color="success"
                                    label="Check All"
                            />
                        </div>
                        <div>{{ remaining }} items left</div>
                    </div>
                    <div>
                        <v-btn
                                @click="filter = 'all'"
                                small>All
                        </v-btn>
                        <v-btn
                                @click="filter = 'active'"
                                class="mx-5"
                                small
                        >Active
                        </v-btn>
                        <v-btn
                                @click="filter = 'completed'"
                                small>Completed
                        </v-btn>
                        <v-fade-transition>
                            <v-btn
                                    @click="clearCompleted"
                                    small
                                    class="mx-5"
                                    v-if="showClearCompletedButton">Clear Completed
                            </v-btn>
                        </v-fade-transition>
                    </div>
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
                newTodo: '',
                idForTodo: 3,
                beforeEditCache: '',
                filter: 'all',
                todos: [
                    {
                        'id': 1,
                        'title': 'Finish Vue app',
                        'completed': false,
                        'editing': false,
                    },
                    {
                        'id': 2,
                        'title': 'Learn Vue',
                        'completed': false,
                        'editing': false
                    }
                ]
            };
        },
        methods: {
            addTodo() {
                if (this.newTodo.trim() == 0) {
                    return
                }

                this.todos.push({
                    id: this.idForTodo,
                    title: this.newTodo,
                    completed: false
                });

                this.newTodo = '';
                this.idForTodo++
            },
            removeTodo(index) {
                this.todos.splice((index), 1)
            },
            editTodo(todo) {
                this.beforeEditCache = todo.title;
                todo.editing = true;
            },
            doneEdit(todo) {
                if (todo.title.trim() == '') {
                    todo.title = this.beforeEditCache;
                }
                todo.editing = false
            },
            cancelEdit(todo) {
                todo.title = this.beforeEditCache;
                todo.editing = false;
            },
            checkAllTodos() {
                // TO DOKONCZENIA!
                this.todos.forEach((todo) => todo.completed = event.target);
                // this.todos.forEach((todo) => todo.completed = event.target.inputValue);
            },
            clearCompleted() {
                this.todos = this.todos.filter(todo => !todo.completed)
            }
        },
        computed: {
            remaining() {
                return this.todos.filter(todo => !todo.completed).length
            },
            anyRemaining() {
                return this.remaining != 0
            },
            todosFiltered() {
                if (this.filter == 'all') {
                    return this.todos
                } else if (this.filter == 'active') {
                    return this.todos.filter(todo => !todo.completed)
                } else if (this.filter == 'completed') {
                    return this.todos.filter(todo => todo.completed)
                }
                return this.todos
            },
            showClearCompletedButton() {
                return this.todos.filter(todo => todo.completed).length > 0
            }
        }
    };
</script>

<style lang="css" scoped>
    .completed {
        text-decoration: line-through;
        color: lightgray;
    }
</style>
