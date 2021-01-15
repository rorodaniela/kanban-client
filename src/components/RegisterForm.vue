<template>
    <div class="form-page">
        <div class="container" id="register-page">
                <h4 class="text-center">Register</h4>
                <hr>
                <form @submit.prevent="register">
                    <div class="form-group">
                        <label for="email-register">Email address :</label>
                        <input v-model="input.email" type="email" class="form-control" name="email-register" id="email-register" placeholder="Enter email" required>
                    </div>
                    <div class="form-group">
                        <label for="password-register">Password :</label>
                        <input v-model="input.password" type="password" class="form-control" name="password-register" id="password-register" placeholder="Password" required>
                    </div>
                    <br>
                    <div>
                        <button type="submit" class="btn btn-primary" id="register-post-btn">submit</button>
                    </div><br>
                </form>
                <button @click.prevent="backLogin" class="btn btn-primary" id="back-login-btn">login</button>
            </div>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    name: "RegisterForm",
    data() {
        return {
            input: {
                email: '', 
                password: ''
            },
            server: 'https://kanban-app-ku.herokuapp.com'
        }
    },
    methods: {
        register() {
            axios({
            method: 'POST',
            url: this.server + '/register',
            data: {
                email: this.input.email,
                password: this.input.password
            }
            })
            .then( response => {
                return this.$emit('changePage', 'login')
            })
            .catch( err => {
                console.log(err);
            })
        }, 
        backLogin() {
            this.$emit('changePage', 'login')
        }
    }
};
</script>

<style></style>
