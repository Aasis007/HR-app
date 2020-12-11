<template>
    <article>
        <div class="container" :class="{'sign-up-active' : signUp}">
            <div class="overlay-container">
                <div class="overlay">
                    <div class="overlay-left">
                        <h2>Welcome Back!</h2>
                        <p>Please login with your personal info</p>
                        <button class="invert" id="signIn" @click="signUp = !signUp">Sign In</button>
                    </div>
                    <div class="overlay-right">
                        <h3>Don't Have an Account?</h3>
                        <p>Please Register</p>
                        <button class="invert" id="signUp" @click="signUp = !signUp">Sign Up</button>
                    </div>
                </div>
            </div>
            //registration from
            <form  @submit="registerData" method="post" class="sign-up" action="#">
                <h2>Sign Up</h2>
                <label>
                    <input type="text" placeholder="Name" name="Name" v-model="register.name"/>
                </label>
                <label>
                    <input type="email" placeholder="Email" name="Email" v-model="register.email" />
                </label>
                <label>
                    <input type="password" placeholder="Password" name="Password" v-model="register.password"/>
                </label>
                <label>
                    <input type="password" placeholder="confirm Password" />
                </label>
                <p style="font-size:12px">Please Upload your Company Document</p>
                <input type="file" id="file" ref="file" v-on:change="fileupload($event)"/>
                <button type="submit" onclick="">Sign Up</button>
            </form>
            //login form
            <form  @submit="loginData" method="post" class="sign-in" action="#">
                <h2>Sign In</h2>
                <div>Use your account</div>
                <label>
                    <input type="email" placeholder="Email" name="email" v-model="login.email" />
                </label>
                <label>
                    <input type="password" placeholder="Password" name="password" v-model="login.password"/>
                </label>
                <a href="#">Forgot your password?</a>
                <button type="submit"  v-on:click="navigate()">Login</button>

            </form>

        </div>
    </article>
</template>

<script>

    import axios from 'axios'
    import VueAxios from 'vue-axios'

    import * as Vue from 'vue';
    import router from "../router";
    import App from "../App";
    const app = Vue.createApp(App)
    app.use(VueAxios,axios,router)



    export default {
        data:() => {
            return {
                signUp: false,
                login:{
                    email: null,
                    password: null
                },

                register: {
                    image:'',
                    name: null,
                    email: null,
                    password: null

                },



            }

        } ,







        methods: {

             loginData(e)
             {
                 let formData = {

                             'email' : this.login.email,
                             'password' : this.login.password
                         };

                 axios.post("http://darlagyomylord.com/hr_project/public/api/web_adminLogin", formData,
                 {
                     headers: {
                         'x-api-key': 'w@i76r_7l@h',
                     },
                 },
                 console.log(this.login)

                     )
                 .then((result)=>{
                    this.login = result.data;
                    console.log(result.data)
                     const loginstate = result.data.message
                     // alert(loginstate);
                     console.warn(loginstate);






             })
                    e.preventDefault()


             },
            navigate() {
                this.$router.push('/dashboard');
            },



            registerData(e)
            {

                let formData = new FormData();
                formData.append('image', this.file);
                formData.append('name',this.register.name);
                formData.append('email',this.register.email);
                formData.append('password',this.register.password);
                //
                // let formData = {
                //     'image' : this.file,npm
                //     'name' : this.register.name,
                //     'email' : this.register.email,
                //     'password' : this.register.password
                // };

                axios.post("http://darlagyomylord.com/hr_project/public/api/save_admin_user",
                    formData,

                    {
                        headers: {
                            'x-api-key': 'w@i76r_7l@h',
                            'Content-Type': 'application/json'
                        },
                    },
                )

                .then((response) => {
                    this.register = response.data;
                    console.log(response.data)
                    const registermsg = response.data.message;
                    alert(registermsg);


                })

                e.preventDefault()

            },

                fileupload(event) {
                 this.file = event.target.files[0];


                }

             }



    }
</script>

<style lang="scss" scoped>
    .container {
        position: relative;
        width: 768px;
        height: 480px;
        border-radius: 10px;
        overflow: hidden;
        box-shadow: 0 15px 30px rgba(0, 0, 0, .2),
        0 10px 10px rgba(0, 0, 0, .2);
        background: linear-gradient(to bottom, #efefef, #ccc);
        .overlay-container {
            position: absolute;
            top: 0;
            left: 50%;
            width: 50%;
            height: 100%;
            overflow: hidden;
            transition: transform .5s ease-in-out;
            z-index: 100;
        }
        .overlay {
            position: relative;
            left: -100%;
            height: 100%;
            width: 200%;
            background: linear-gradient(to bottom right, #0e49b5, #009345);
            color: #fff;
            transform: translateX(0);
            transition: transform .5s ease-in-out;
        }
        @mixin overlays($property) {
            position: absolute;
            top: 0;
            display: flex;
            align-items: center;
            justify-content: space-around;
            flex-direction: column;
            padding: 70px 40px;
            width: calc(50% - 80px);
            height: calc(100% - 140px);
            text-align: center;
            transform: translateX($property);
            transition: transform .5s ease-in-out;
        }
        .overlay-left {
            @include overlays(-20%);
        }
        .overlay-right {
            @include overlays(0);
            right: 0;
        }
    }
    h2 {
        margin: 0;
    }
    p {
        margin: 20px 0 30px;
    }
    a {
        color: #222;
        text-decoration: none;
        margin: 15px 0;
        font-size: 1rem;
    }
    button {
        border-radius: 20px;
        border: 1px solid #009345;
        background-color: #009345;
        color: #fff;
        font-size: 1rem;
        font-weight: bold;
        padding: 10px 40px;
        letter-spacing: 1px;
        text-transform: uppercase;
        margin-top: 8px;
        cursor: pointer;
        transition: transform .1s ease-in;
        &:active {
            transform: scale(.9);
        }
        &:focus {
            outline: none;
        }
    }
    button.invert {
        background-color: transparent;
        border-color: #fff;
    }
    form {
        position: absolute;
        top: 0;
        display: flex;
        align-items: center;
        justify-content: space-around;
        flex-direction: column;
        padding: 90px 60px;
        width: calc(50% - 120px);
        height: calc(115% - 182px);
        text-align: center;
        background: linear-gradient(to bottom, #efefef, #ccc);
        transition: all .5s ease-in-out;
        div {
            font-size: 1rem;
        }
        input {
            background-color: #eee;
            border: none;
            padding: 8px 15px;
            margin: 4px 0;
            width: calc(100% - 30px);
            border-radius: 15px;
            border-bottom: 1px solid #ddd;
            box-shadow: inset 0 1px 2px rgba(0, 0, 0, .4),
            0 -1px 1px #fff,
            0 1px 0 #fff;
            overflow: hidden;
            &:focus {
                outline: none;
                background-color: #fff;
            }
        }
    }
    .sign-in {
        left: 0;
        z-index: 2;
    }
    .sign-up {
        left: 0;
        z-index: 1;
        opacity: 0;
    }
    .sign-up-active {
        .sign-in {
            transform: translateX(100%);
        }
        .sign-up {
            transform: translateX(100%);
            opacity: 1;
            z-index: 5;
            animation: show .5s;
        }
        .overlay-container {
            transform: translateX(-100%);
        }
        .overlay {
            transform: translateX(50%);
        }
        .overlay-left {
            transform: translateX(0);
        }
        .overlay-right {
            transform: translateX(20%);
        }
    }
    @keyframes show {
        0% {
            opacity: 0;
            z-index: 1;
        }
        49% {
            opacity: 0;
            z-index: 1;
        }
        50% {
            opacity: 1;
            z-index: 10;
        }
    }
</style>
