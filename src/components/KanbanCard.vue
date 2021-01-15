<template>
    <div>
        <Navbar @logout="logout" @changePage="changePage"></Navbar>
        <div class="container">    
            <div class="row">
                <!-- backlog -->
                <div class="col-3" v-for="category in categories" :key="category.id">
                    <div class="card text-white bg-danger mb-3" style="max-width: 18rem;">
                        <div class="card-header text-center">{{ category.category }}</div>
                        <div class="card-body">
                            <!-- isi task -->
                            <TaskCard 
                            v-for="task in category.Tasks" :key="task.id" :dataTask="task" 
                            @changeCurrentTask="changeCurrentTask"
                            @changePage="changePage"
                            ></TaskCard>
                        </div>
                    </div>  
                </div>
            </div>
        </div>
    <div>
</template>

<script>
import TaskCard from './TaskCard'
import Navbar from './Navbar'

export default {
    name: 'KanbanCard',
    props: ['categories'],
    components: {
        TaskCard,
        Navbar
    },
    data() {
        return {

        }
    },
    methods: {
        logout(){
            return this.$emit('logout')
        },
        changePage(page) {
            return this.$emit('changePage', page)
        },
        changeCurrentTaskId(id) {
            this.$emit('changeCurrentTaskId', id)
        },
        changeCurrentTask(data){
            this.$emit('changeCurrentTask', data)
        }
    }
}
</script>

<style>

</style>