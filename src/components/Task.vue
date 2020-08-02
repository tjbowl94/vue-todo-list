<template>
    <div class="task">
        <p>Order: {{task.order}}</p>
        <div v-bind:class="completedClass">
            <input type="checkbox" v-model="task.completed" />
            <div v-if="editing">
                <input type="text" v-model="task.description" />
            </div>
            <div v-else>
                {{task.description}}
            </div>
        </div>
        <button v-on:click="toggleEditing">Edit Task</button>
        <button v-on:click="deleteTask">Delete Task</button>
    </div>
</template>


<script>
    export default {
        name: 'task',
        props: {
            task: Object
        },
        data: function() {
            return {
                editing: false
            };
        },
        computed: {
            completedClass: function() {
                return {
                    completed: this.task.completed
                };
            }
        },
        methods: {
            deleteTask: function() {
                this.$emit('delete-task', this.task.id);
            },
            toggleEditing: function() {
                this.editing = !this.editing;
            }
        }
    }
</script>


<style scoped>
    .task {
        border: 1px solid black;
        width: 250px;
        margin: auto;
        margin-bottom: 25px;
    }
    .completed {
        text-decoration: line-through;
    }
</style>
