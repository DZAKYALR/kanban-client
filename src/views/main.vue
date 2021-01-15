<template>
<div class="row overflow-scroll" style="height: 100vh; background-color: #f8f9fa;"  >
    <Category
        v-for="category in categories"
        :key="category.id"
        :category='category'
        :categories='categories'
        @getKanban='getKanban'
        @getCategory='getCategory'
        @getUser='getUser'
        @addTask='addTask'
        @changeKanbanPage='changeKanbanPage'
        @delTask='delTask'
        @delCategory='delCategory'
        @editTask='editTask'
        @moveCat='moveCat'
        :tasks='tasks'
        :user='user'
    ></Category>
</div>

</template>

<script>
import Category from "../components/category"

export default {
    name: 'Main',
    data () {
        return {
            
        }
    },
    components: {
        Category
    },
    props: ['categories', 'tasks','user'],
    methods: {
        getKanban () {
            this.$emit('getKanban')
        },
        getCategory () {
            this.$emit('getCategory')
        },
         getUser () {
            this.$emit('getUser')
        },
        addTask (payload) {
            this.$emit('addTask')
        },
        changeKanbanPage (page) {
            this.$emit('changeKanbanPage',page)
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
    created () {
        this.getKanban() 
        this.getCategory() 
        this.getUser() 
    }
}

</script>

<style>

</style>