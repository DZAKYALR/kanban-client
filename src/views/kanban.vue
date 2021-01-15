<template>
    <!--Kanban-->
            <div>
                <Navbar
                    @logout="logout"
                    @addCategory="addCategory"
                    @changePage="changePage"
                    @changeKanbanPage='changeKanbanPage'
                ></Navbar>  
            <!--Navbar-->
                
            <!--Main Menu-->
                <Main
                    v-if="pageKanban === 'Main'"
                    @getKanban='getKanban'
                    @getCategory='getCategory'
                    @getUser='getUser'
                    @addTask='addTask'
                    @addCategory='addCategory'
                    @changeKanbanPage='changeKanbanPage'
                    @delTask='delTask'
                    @delCategory='delCategory'
                    @editTask='editTask'
                    @moveCat='moveCat'
                    :categories='categories'
                    :tasks='tasks'
                    :user='user'
                ></Main>
            <!--Add Task-->
                <AddTask 
                    v-if="pageKanban === 'AddTask'"
                    @addTask='addTask'
                    :categories='categories'
                    @changeKanbanPage='changeKanbanPage'
                ></AddTask>
            <!--Add Category-->
                <AddCategory
                    v-if="pageKanban === 'AddCategory'"
                    @addCategory='addCategory'
                    @changeKanbanPage='changeKanbanPage'
                >
                </AddCategory>
           
                
            </div>
</template>

<script>
import Navbar from "../components/navbar"
import Main from "../views/main"
import AddTask from "../components/addTask"
import AddCategory from "../components/addCategory"


export default {
    name: 'Kanban',
    data () {
        return {
            pageKanban : 'Main',
            selectedTask : []
        }
    },
    props: ['categories', 'tasks','user'],
    methods: {
        logout () {
            this.$emit('logout')
        },
        getKanban () {
            this.$emit('getKanban')
        },
        getCategory () {
            this.$emit('getCategory')
        },
        getUser () {
            this.$emit('getUser')
        },
        addCategory (name) {
            this.$emit('addCategory',name)
        },
        addTask (payload) {
            this.$emit('addTask', payload)
        },
        changePage (page) {
            this.$emit('changePage', page)
        },
        changeKanbanPage (page) {
            this.pageKanban = page
        },
        delTask (payload) {
            this.$emit('delTask', payload)
        },
        delCategory (payload) {
            this.$emit('delCategory', payload)
        },
        editTask (payload) {
            this.$emit('editTask', payload)
        },
        moveCat (payload) {
            this.$emit('moveCat', payload)
        }
    },
    components: {
        Navbar,
        Main,
        AddTask,
        AddCategory
    },
    created () {
        this.menu = 'main'
        this.getKanban() 
        this.getCategory() 
        this.getUser() 
    }
}
</script>

<style>

</style>