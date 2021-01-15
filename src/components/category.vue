<template>
    <!--Kanban Category-->
    <div class="col" style="background-color: #e8eae6" > 
        <div class="card" style="background-color: #cfdac8;">
            <div class="card-header">
                <div class="row card-title ">
                    <div 
                        class="col-10 m-0" 
                        style="text-align-last: center; border-bottom: groove; font-size: x-large"
                        >{{ category.name }}
                        </div>
                    <div 
                        class="col-2"
                        style="border-bottom: groove">
                        <ul class="list-inline m-0">
                            <li class="list-inline-item">
                                <button 
                                    class="btn btn-danger btn-sm rounded-0" 
                                    type="button"  
                                    title="Delete" 
                                    @click="delCategory(category.id)">
                                        <i class="bi bi-trash" >
                                        </i>X
                                </button>
                            </li>
                        </ul>
                    </div>
                </div>
                <!--Kanban Task-->
                <TaskCard
                    v-for="task in filteredTasks" 
                    :key="task.id"
                    :task='task'
                    :user='user'
                    :categories='categories'
                    @getUser='getUser'
                    @editTask='editTask'
                    @delTask='delTask'
                    @moveCat='moveCat'
                ></TaskCard>
            </div>
        <button 
            type="button" 
            class="btn "
            style="background-color: #cdd0cb " 
            @click="changeKanbanPage('AddTask')">Add Task
        </button>
    </div>
</template>

<script>
import TaskCard from "./taskCard"
import Swal from 'sweetalert2'

export default {
    name: 'Category',
    components: {
        TaskCard
    },
    data () {
        return {
            name: '',
            selectedCategoryId: 0,
            componentKey: 0,
        }
    },
    props: ['category', 'tasks', 'user', 'categories'],
    methods: {
        changeKanbanPage (page) {
            this.$emit('changeKanbanPage', page)
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
        getById (val) {
            return this.user.filter(user => user.id === val)[0].email
        },
        delTask (payload) {
            this.$emit('delTask', payload)
        },
        delCategory (id) {
            const swalWithBootstrapButtons = Swal.mixin({
                customClass: {
                    confirmButton: 'btn btn-success',
                    cancelButton: 'btn btn-danger'
                },
                buttonsStyling: false
                })

                swalWithBootstrapButtons.fire({
                title: 'Are you sure?',
                text: "You won't be able to revert this!",
                icon: 'warning',
                showCancelButton: true,
                confirmButtonText: 'Yes, delete it!',
                cancelButtonText: 'No, cancel!',
                reverseButtons: true
                }).then((result) => {
                if (result.isConfirmed) {
                    swalWithBootstrapButtons.fire(
                    'Deleted!',
                    'Your category has been deleted.',
                    'success'
                    )
                    this.$emit('delCategory', {id:id})
                } else if (
                    /* Read more about handling dismissals below */
                    result.dismiss === Swal.DismissReason.cancel
                ) {
                    swalWithBootstrapButtons.fire(
                    'Cancelled',
                    'Your category is safe :)',
                    'error'
                    )
                }
            })            
            
        },
        editTask (payload) {
            this.$emit('editTask', payload)
        },
        moveCat (payload) {
            this.$emit('moveCat', payload)
        },
        
    },
    computed: {
        filteredTasks () {
            return this.tasks.filter(task => task.categoryId === this.category.id)
        },
    },
    created() {
        this.getKanban() 
        this.getCategory() 
        this.getUser() 
    }
}
</script>

<style>

</style>

