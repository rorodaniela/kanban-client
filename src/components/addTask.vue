<template>
    <div class="form-page"> 
        <div class="container" id="add-task-page">
            <h3 class="text-center">Add New Task</h3>
            <hr>
            <form @submit.prevent="addTask">
                <div class="form-group">
                    <label for="title-add">Title :</label>
                    <input v-model="input.title" type="text" class="form-control" name="title-add" id="title-add" placeholder="Enter title" required>
                </div>
                <div class="form-group">
                    <label for="description-add">Description :</label>
                    <input v-model="input.description" type="text" class="form-control" name="description-add" id="description-add" placeholder="Enter description" required>
                </div>
                <div class="form-group">
                    <label for="asignTo-add">Assign To :</label>
                    <input v-model="input.asignTo" type="text" class="form-control" name="asignTo-add" id="asignTo-add" placeholder="Assign To" required>
                </div>
                <div>
                    <br>
                    <button type="submit" class="btn btn-primary" id="add-post-btn ">Add</button>
                </div>
            </form>
        </div>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    name: 'AddTask',
    data() {
        return {
            input : {
                title: '',
                description: '',
                asignTo: ''
            },
            server: 'https://kanban-app-ku.herokuapp.com'
        }
    },
    methods: {
        addTask() {
            axios({
            method: 'POST',
            url: this.server + '/tasks',
            data: {
                title: this.input.title,
                description: this.input.description,
                asignTo: this.input.asignTo
            },
            headers: {
                access_token: localStorage.access_token
            }
            })
            .then( response => {
                return this.$emit('addTask', 'task')
            })
            .catch( err => {
                console.log(err);
            })
        }
    }
}
</script>

<style>

</style>