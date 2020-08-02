<template>
    <div>
        <h1>To-do list</h1>
        <div>
            <draggable
                v-model="tasks"
                group="tasks"
                :move="onMove"
                @start="drag=true"
                @end="handleDrag"
            >
                <task 
                    v-for="task in tasks" 
                    v-bind:key="task.id" 
                    v-bind:task="task"
                    v-on:delete-task="deleteTask"
                />
            </draggable>
        </div>
        <br />
        <add-task v-on:add-task="addTask" />
    </div>
</template>

<script>
    import Task from './Task';
    import AddTask from './AddTask';
    import draggable from 'vuedraggable';

    export default {
        name: 'list',
        data: function() {
            return {
                tasks: [
                    {
                        id: 1,
                        description: 'Build an app in vue!',
                        completed: false,
                        order: 1
                    },
                    {
                        id: 2,
                        description: 'Read a good book',
                        completed: false,
                        order: 2
                    }
                ],
                dragging: false
            }
        },
        components: {
            AddTask,
            Task,
            draggable
        },
        methods: {
            addTask: function(description) {
                let id = 1;
                let order = 1;

                if (this.tasks.length > 0) {
                    id = Math.max.apply(Math, this.tasks.map(t => t.id)) + 1;
                    order = Math.max.apply(Math, this.tasks.map(t => t.order)) + 1;
                }

                this.tasks = [
                    ...this.tasks,
                    {
                        id: id,
                        description: description,
                        completed: false,
                        order: order
                    }
                ];
            },
            deleteTask: function(id) {
                let index = this.tasks.findIndex(t => t.id === id);

                if (index !== -1) {
                    let tasksCopy = [...this.tasks];
                    tasksCopy.splice(index, 1);
                    this.tasks = tasksCopy;
                    this.reorderTasks();
                }
            },
            reorderTasks: function() {
                for (var i = 0; i < this.tasks.length; i++) {
                    this.tasks[i].order = i + 1;
                }
            },
            handleDrag: function() {
                this.dragging = false;
                this.reorderTasks();
            }
        }
    }
</script>

<style scoped>

</style>
