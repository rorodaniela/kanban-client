<template>
    <div>
        <LoginForm
            v-if="currentPage == 'login'"
            @login="isLogin"
            @register="changePage"
        ></LoginForm>
        <RegisterForm
            v-else-if="currentPage == 'register'"
            @changePage="changePage"
        ></RegisterForm>
        <KanbanCard
            v-else-if="currentPage == 'task'"
            @logout="checkAuth"
            :categories="categories"
            @changePage="changePage"
            @changeCurrentTask="changeCurrentTask"
        ></KanbanCard>
        <AddTask
            v-else-if="currentPage == 'addTask'"
            @addTask="addTask"
        ></AddTask>
        <Update
            v-else-if="currentPage == 'update'"
            :currentTask="currentTask"
            :categories="categories"
            @changePage="changePage"
        ></Update>
        <StatusTask 
            v-else-if="currentPage == 'status'"
            :currentTask="currentTask"
            :categories="categories"
            @updateStatus="updateStatus"
        ></StatusTask>
    </div>
</template>

<script>
import LoginForm from "./components/LoginForm.vue";
import RegisterForm from "./components/RegisterForm.vue";
import KanbanCard from "./components/KanbanCard";
import AddTask from "./components/addTask";
import Update from "./components/Update";
import StatusTask from "./components/StatusTask"
import axios from "axios";

export default {
    name: "App",
    data() {
        return {
            server: "https://kanban-app-ku.herokuapp.com",
            isLoggedIn: false,
            categories: [],
            currentPage: "login",
            currentTask: null,
        };
    },
    components: {
        LoginForm,
        RegisterForm,
        AddTask,
        KanbanCard,
        Update,
        StatusTask
    },
    methods: {
        changePage(page) {
            console.log(page, "qweeeee");
            this.currentPage = page;
        },
        checkAuth() {
            if (localStorage.access_token) {
                this.fetchTask();
                this.isLoggedIn = true;
                this.changePage("task");
            } else {
                this.isLoggedIn = true;
                this.changePage("login");
            }
        },
        isLogin(status) {
            console.log(status, "<<< ini status dari app.vue >>>");
            this.isLoggedIn = status;
            this.fetchTask();
            this.changePage("task");
        },
        fetchTask() {
            axios({
                method: "GET",
                url: this.server + "/categories",
                headers: {
                    access_token: localStorage.access_token,
                },
            })
                .then((response) => {
                    this.categories = response.data;
                })
                .catch((err) => {
                    console.log(err);
                });
        },
        addTask(page) {
            this.fetchTask();
            this.changePage(page);
        },
        changeCurrentTask(data){
            this.currentTask = data
        },
        updateTask(page){
            console.log(page, "ini dari update task yaa");
        },
        updateStatus(page){
            console.log(page, "ini dari update status yaa");
        }
    },
    created() {
        this.checkAuth();
    },
};
</script>

<style></style>
