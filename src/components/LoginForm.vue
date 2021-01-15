<template>
    <div class="form-page">
        <div class="container" id="login-page">
            <h4 class="text-center">Login</h4>
            <hr />
            <form @submit.prevent="login">
                <div class="form-group">
                    <label>Email address :</label>
                    <input
                        v-model="input.email"
                        type="text"
                        name="email-login"
                        id="email-login"
                        class="form-control"
                        placeholder="Input your email"
                    />
                    <label>Password :</label>
                    <input
                        v-model="input.password"
                        type="password"
                        name="password-login"
                        id="password-login"
                        class="form-control"
                        placeholder="Input your password"
                    />
                </div>
                <br />
                <button type="submit" class="btn btn-primary" id="login-btn">
                    login
                </button>
                <button
                    type="submit"
                    class="btn btn-danger"
                    id="register-btn"
                    @click="register"
                >
                    register
                </button>
                <br />
            </form>
            <p class="text-center">Or sign in with google</p>
            <button
                v-google-signin-button="clientId"
                class="google-signin-button btn btn-primary"
            >
                Continue with Google
            </button>
        </div>
    </div>
</template>

<script>
import axios from "axios";
import GoogleSignInButton from "vue-google-signin-button-directive";
import Swal from "sweetalert2";

export default {
    name: "LoginForm",
    data() {
        return {
            input: {
                email: "",
                password: "",
            },
            server: "https://kanban-app-ku.herokuapp.com",
            clientId:
                "834629336183-e2ofd3t0mggc2duv7tbsvlfunkidom9i.apps.googleusercontent.com",
        };
    },
    methods: {
        login() {
            if (!this.input.email || !this.input.password) {
                Swal.fire({
                    icon: "error",
                    title: "Login failed",
                    text: "Please enter your email and password!"
                });
            } else {
                axios({
                    method: "POST",
                    url: this.server + "/login",
                    data: {
                        email: this.input.email,
                        password: this.input.password,
                    },
                })
                    .then((response) => {
                        localStorage.access_token = response.data.access_token;
                        return this.$emit("login", true);
                    })
                    .catch((err) => {
                        Swal.fire({
                            icon: "error",
                            title: "Oops..."
                        });
                        console.log(err);
                    });
            }
        },
        register() {
            return this.$emit("register", "register");
        },
        OnGoogleAuthSuccess(idToken) {
            // let id_token = idToken.getAuthResponse().id_token;
            let id_token = idToken;
            axios({
                method: "POST",
                url: this.server + "/loginGoogle",
                data: {
                    id_token,
                },
            })
                .then((response) => {
                    localStorage.access_token = response.data.access_token;
                    return this.$emit("login", true);
                })
                .catch((err) => {
                    console.log(err);
                });
        },
        OnGoogleAuthFail(error) {
            console.log(error);
        },
    },
};
</script>

<style></style>
