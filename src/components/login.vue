<template>
  <div class="form-login" >
                <div class="login-card">
                    <h1>Log-in</h1><br>
                  <form @submit.prevent="login">
                    <input v-model="email" type="text" name="email" placeholder="Email">
                    <input v-model="password" type="password" name="pass" placeholder="Password">
                    <input type="submit" name="login" class="login login-submit" value="Login">
                  </form> 
                    
                  <div class="login-help" style="cursor:grab">
                    <a  @click="changePage('register')">Register</a> 
                  </div>
                  <hr>
                  <div>
                      <p class="login-help" >Or Sign</p>
                  </div>
                  <div style="float:center">
                      <GoogleLogin :params="params" :renderParams="renderParams" :onSuccess="onSuccess" :onFailure="onFailure"></GoogleLogin>
                  </div>
                </div>
            </div>
</template>

<script>
import GoogleLogin from 'vue-google-login';

export default {
    name: 'LoginForm',
    data () {
        return {
            email: '',
            password: '',
            params: {
                    client_id: "171212649923-7bjgf723ekk2jtcf16jpoesviq75v8l4.apps.googleusercontent.com"
            },
            renderParams: {
                    width: 250,
                    height: 50,
                    longtitle: true
                }
        }
    },
    components: {
            GoogleLogin
    },
    methods: {
        login () {
            this.$emit('login', {
                email: this.email,
                password: this.password
            })
        },
        changePage (val) {
            this.$emit('changePage', val)
        },
        onSuccess(googleUser) {
            this.onSignIn(googleUser)
            // This only gets the user information: id, name, imageUrl and email
        },
        onFailure(err) {
            console.log(err);
        },
        onSignIn (googleUser) {
            this.$emit('onSignIn', googleUser)
        }
    } 

}
</script>

<style>

</style>