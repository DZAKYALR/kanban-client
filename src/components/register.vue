<template>
  <div class="form-register" >
                <div class="register-card">
                    <h1>Register</h1><br>
                  <form @submit.prevent='register'>
                    <input v-model="email" type="text" name="email" placeholder="Email">
                    <input v-model="password" type="password" name="pass" placeholder="Password">
                    <input type="submit" name="register" class="register register-submit" value="Register">
                  </form>
                    
                  <div class="register-help">
                    <a href="#" @click="changePage('login')">Log In</a> 
                  </div>
                </div>
            </div>
</template>

<script>
export default {
    name: 'RegisterForm',
    data () {
        return {
            email: '',
            password: ''
        }
    },
    methods: {
        register () {
            this.$emit('register', {
              email: this.email,
              password: this.password
            })
        },
        changePage (page) {
            this.$emit('changePage', page)
        }
    } 
}
</script>

<style>

</style>