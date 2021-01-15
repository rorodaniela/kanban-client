<template>
    <div >
        <div class="card text-white mb-3" style="max-width: 18rem;">
            <div class="card-body bg-white">
                <h4 class="card-title text-center">{{dataTask.title}}</h4>
                <p class="card-text">Desc : {{dataTask.description}}</p>
                <p class="card-text">Asign To : {{dataTask.asignTo}}</p>
                <button @click.prevent="update" class="btn btn btn-outline-primary d-grid gap-2">Update</button> 
                <button @click.prevent="status" class="btn btn btn-outline-primary d-grid gap-2">Status</button>  
                <button @click.prevent="deleteTask" class="btn btn btn-outline-primary d-grid gap-2">Delete</button>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    name: 'TaskCard',
    props: ['dataTask'],
    data() {
        return {

        }
    },
    methods: {
        update() {
            let task = {
                id: this.dataTask.id,
                title: this.dataTask.title,
                description: this.dataTask.description,
                asignTo: this.dataTask.asignTo
            }
            this.$emit('changeCurrentTask', task)
            this.$emit('changePage', 'update')
        },
        status() {
             let task = {
                id: this.dataTask.id,
                title: this.dataTask.title,
                description: this.dataTask.description,
                asignTo: this.dataTask.asignTo
            }
            this.$emit('changeCurrentTask', task)
            this.$emit('changePage', 'status')
        },
        deleteTask() {
            axios({
                method: "DELETE",
                url: "http://localhost:3000/tasks/" + this.dataTask.id,
                headers : {
                    access_token : localStorage.access_token
                }
            })
                .then((response) => {
                    this.$emit("changePage", "task");
                })
                .catch((err) => {
                    console.log(err);
                });
        }

        
    }
}
</script>

<style>

</style>