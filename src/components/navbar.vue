<template>
    <div class="navbar" style="background-color:#7c9473" >
        <h1 style="color:#343a40; padding:1vh" 
            @click="changeKanbanPage('Main')"
            >Kanban</h1>
            <div class="nav-right" >
                <div class="btn bg-white" style="border-right: outset"  @click="changeKanbanPage('AddCategory')">Add Category</div>
                <div class="btn bg-white" style="border-right: outset" @click="logout()" :logoutButton=true>Log-Out</div>

            </div>
        </div>
</template>

<script>
import GoogleLogin from 'vue-google-login';
export default {
    name: 'Navbar',
    methods: {
        logout () {
            this.$emit('logout')
        },
        addCategory () {
            this.$emit('addCategory')
        },
        changePage (page) {
            this.$emit('changePage', page)
        },
        changeKanbanPage (page) {
            this.$emit('changeKanbanPage', page)
        }

    },
     components: {
            GoogleLogin
    },
}
</script>

<style>
    
</style>