<template>
  <div>
      <div class="login-card">
            <h1>Add Task</h1><br>
            <form
                @submit.prevent="addTask"
                style="display: flex; flex-direction: column">
                <div class="mb-3">
                    
                <div class="mb-3">
                    <label for="Name-add" class="form-label">Name</label>
                        <input
                        v-model="name"
                        type="text"
                        class="form-control"
                        id="Name-add">
                </div>

                <div class="mb-3">
                    <label for="category-add" class="form-label">Category</label>
                        <select
                            v-model="categoryId"
                            id="category-add"
                            class="form-select"
                            aria-label="category-add">
                                <option 
                                    v-for="name in categories" 
                                    :key="name.id" 
                                    :value="name.id" 
                                    >{{name.name}}
                                </option>
                        </select>
                </div>

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
    name: 'AddTask',
    data () {
        return {
            name: '',
            categoryId: 0
        }
    },
    props : ['categories'],
    methods: {
        changeKanbanPage (page) {
            this.$emit('changeKanbanPage',page)
        },
        addTask () {
            this.$emit('addTask', {
                name: this.name,
                categoryId: this.categoryId
            })
            this.changeKanbanPage('Main')
        },
    }
}
</script>

<style>

</style>