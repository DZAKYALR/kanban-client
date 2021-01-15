<template>
  <div>
      <div class="register-card">
        <h1>Add Category</h1><br>
            <form @submit.prevent="addCategory">
            <input 
                v-model="name" 
                type="text" 
                name="Category" 
                placeholder="Input New Category">
            <button
                type="submit"
                class="btn btn-primary align-self-stretch mt-2"
                >Create
            </button> 
            <button
                type="button"
                @click="changeKanbanPage('Main')"
                class="btn btn-danger align-self-stretch mt-2"
                >Cancel
            </button>
            </form>
            
        </div>
  </div>
</template>

<script>
export default {
    name: 'AddCategory',
    data () {
        return {
            name: '',
        }
    },
    methods: {
        changeKanbanPage (page) {
            this.$emit('changeKanbanPage',page)
        },
        addCategory () {
            this.$emit('addCategory', {
                name: this.name})
            this.changeKanbanPage('Main')
        },
    }
}
</script>

<style>

</style>