<template>
<div>
    <h1>welcome to our todolist</h1>
    <form @submit.prevent="validate">
        <input required type="text" v-model="todos.item" />
        <input type="submit" />
    </form>
    <h3>Number of Todos: {{Totaltodos}}</h3>
    <ol v-if="ContainTodos">
        <li v-for="todoItem in todoItems" :key="todoItem"> {{todoItem}}</li>
    </ol>
    <p v-else> You have no todos .kindly add some items</p>
</div>
</template>

<script>

export default {
    name: 'Todo',
    data() {
        return {
            Totaltodos:null,
            todoItems: [],
            todos: {
                item: null,
            }

        }
    },
    computed: {
        ContainTodos: function () {
            return this.todoItems.length > 0;
        },

    },
    methods: {
        validate: function () {
            this.todoItems.push(this.todos.item);
            this.Totaltodos=this.todoItems.length;
            this.resetFields();
        },
        resetFields: function () {
            this.todos.item = '';
        }
    },
    created() {

    }
}
</script>

<style scoped>

</style>
