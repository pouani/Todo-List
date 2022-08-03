<template lang="">
    <section class="todoapp">
        <header class="header">
            <h1>Todos List</h1>
            
            <input type="text" class="new-todo" placeholder="Ajouter une tache" 
            v-model="newTodo" @keyup.enter="addTodo" 
            />
        </header>
        <div class="main">
            <input type="checkbox" class="toggle-all" v-model="allDone"/>
            <ul class="todo-list">
                <li class="toto" v-for="todo in filteredTodos" :key="todo"
                :class="{ completed : todo.completed, editing: todo === editing }">
                    <div class="view">
                        <input type="checkbox" v-model="todo.completed" class="toggle"/>
                        <label @dblclick="editTodo(todo)">{{ todo.name }}</label>
                        <button class="destroy" @click.prevent="deleteTodo(todo)"></button>
                    </div>
                    <input type="text" class="edit" v-model="todo.name" @keyup.enter="doneEdit"
                    @blur="doneEdit" v-focus="todo === editing"/>
                </li>
            </ul>
        </div>
        <footer class="footer" v-show="todos.length > 0">
            <span class="todo-count"><strong>{{ remaining }}</strong> Taches à faires</span>
            <ul class="filters">
                <li>
                    <a href="#" :class="{selected: filter == 'all' }" 
                    @click.prevent="filter = 'all'">Toutes</a>
                </li>
                <li>
                    <a href="#" :class="{selected: filter == 'todo' }" 
                    @click.prevent="filter = 'todo'">A faire</a>
                </li>
                <li>
                    <a href="#" :class="{selected: filter == 'done' }" 
                    @click.prevent="filter = 'done'">Faite</a>
                </li>
            </ul>
            <button class="clear-completed" v-show="completed"
            @click.prevent="deleteCompleted">Supprimer taches finis</button>
        </footer>
    </section>
</template>
<script>
// import { nextTick } from 'process'
import { nextTick } from 'vue'
export default {
    data() {
        return {

            allDone: false,

            todos: [{
                name: 'Tache de test',
                completed: false
            }],
            newTodo: '',
            filter: 'all',
            editing: null,
        }
    },
    methods: {
        addTodo(){
            this.todos.push({
                completed: false,
                name: this.newTodo
            })
            this.newTodo = ''
        },

        deleteTodo (todo){
            this.todos = this.todos.filter(i => i !== todo)
        },
        deleteCompleted(){
            this.todos = this.todos.filter(todo => !todo.completed)
        },

        editTodo(todo){
            this.editing = todo;
        },
        doneEdit(){
            this.editing = null
        }
    }, 
    computed: {
        // allDone: {
        //     get(){
        //         return this.remaining === 0
        //     },
        //     set(value){
        //         this.todos.forEach(todo => {
        //             todo.completed = value
        //         })
        //     }
        // },
        remaining() {
            // this.todos.filter(function(todo) {
            //     return !todo.completed
            // })
            return this.todos.filter(todo => !todo.completed).length
        },
        completed() {
            return this.todos.filter(todo => todo.completed).length
        },
        filteredTodos () {
            if(this.filter === 'todo'){
                return this.todos.filter(todo => !todo.completed)
            } else if(this.filter === 'done'){
                return this.todos.filter(todo => todo.completed)
            }
            return this.todos
        }
    },
    directives: {
        focus(el, value) {
            if(value){
                nextTick(() => {
                     el.focus()
                })
               
            }
        }
    }
}
</script>
<style src="./todo.css">
    
</style>