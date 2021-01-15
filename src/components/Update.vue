<template>
    <div class="form-page">
        <div class="container">
            <h4 class="text-center">Update Task</h4>
            <hr />
            <br />
            <form @submit.prevent="updateTask">
                <div class="form-group">
                    <label for="title-add">Title :</label>
                    <input v-model="input.title" type="text" class="form-control" name="title-add" id="title-add" :placeholder="currentTask.title" required>
                </div>
                <div class="form-group">
                    <label for="description-add">Description :</label>
                    <input v-model="input.description" type="text" class="form-control" name="description-add" id="description-add" :placeholder="currentTask.description" required>
                </div>
                <div class="form-group">
                    <label for="asignTo-add">Assign To :</label>
                    <input v-model="input.asignTo" type="text" class="form-control" name="asignTo-add" id="asignTo-add" :placeholder="currentTask.asignTo" required>
                </div>
                <div>
                    <label>Status :</label>
                    <select v-model="select" class="form-select" aria-label="Default select example">
                        <option v-for="category in categories" :key="category.id" v-bind:value="category.id">{{category.category}}</option>
                    </select>
                    <br>
                    <button type="submit" class="btn btn-primary" id="update-post-btn ">Update</button>
                </div>
            </form>
            <!-- kasih button -->
        </div>
    </div>
</template>

<script>
import axios from "axios"

export default {
    name: "Update",
    props: ["currentTask", "categories"],
    data() {
        return {
            input: {
                title: "",
                description: "",
                asignTo: ""
            },
            select: ""
        };
    },
    methods: {
        updateTask() {
            axios({
                method: "PUT",
                url: "https://kanban-app-ku.herokuapp.com/tasks/" + this.currentTask.id,
                data: {
                    title: this.input.title,
                    description: this.input.description,
                    asignTo: this.input.asignTo,
                    categoryId: this.select
                },
                headers : {
                    access_token : localStorage.access_token
                }
            })
            .then((response) => {
                return this.$emit("changePage", "task");
            })
            .catch((err) => {
                console.log(err);
            });
        }
    },
};
</script>
