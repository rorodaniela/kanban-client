<template>
    <div class="form-page">
        <div class="container">
            <h4 class="text-center">Update Status</h4>
            <hr />
            <br />
            <form @submit.prevent="updateStatus">
                <div>
                    <label>Status :</label>
                    <select v-model="select" class="form-select" aria-label="Default select example">
                        <option v-for="category in categories" :key="category.id" v-bind:value="category.id">{{category.category}}</option>
                    </select>
                    <br>
                    <button type="submit" class="btn btn-primary" id="update-post-btn ">Update</button>
                </div>
            </form>
        </div>
    </div>
</template>

<script>
import axios from "axios"

export default {
    name: "StatusTask",
    props: ['categories', 'currentTask'],
    data() {
        return {
            select: ''
        };
    },
    methods: {
       updateStatus() {
           axios({
                method: "PATCH",
                url: "https://kanban-app-ku.herokuapp.com/tasks/" + this.currentTask.id,
                data: {
                    categoryId: this.select
                },
                headers : {
                    access_token : localStorage.access_token
                }
            })
            .then((response) => {
                return this.$emit("updateStatus", "task");
            })
            .catch((err) => {
                console.log(err);
            });
       }
    },
};
</script>
