<template>
    <div>
        <form v-on:submit.prevent="addTask">
            <label>Description: </label>
            <input type="text" v-model="description" />
            <button>Add New Task</button>
            <span v-if="error">{{error}}</span>
        </form>
    </div>
</template>


<script>
    export default {
        name: "add-task",
        data: function() {
            return {
                description: '',
                error: ''
            };
        },
        computed: {
            validName: function() {
                return this.description !== null && this.description !== "";
            }
        },
        methods: {
            addTask: function() {
                this.error = "";

                if (this.validName) {
                    this.$emit('add-task', this.description);

                    this.description = "";
                } else {
                    this.error = "Please enter a description.";
                }
            }
        }
    }
</script>


<style scoped>

</style>
