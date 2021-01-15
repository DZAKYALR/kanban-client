<template>
 <div 
    class="card-body"
    style="background-color: #cdd0cb" 
    >
    <div class="card mb-0 bg-light">  
        <div class="card-boody p-2"  >
            <div class="float-right mr-n2">
                <p 
                    class="h5" 
                    v-if="!editMode" 
                    @click="editButton"
                    style="cursor: grab;"
                    >{{task.name}}</p>
                <form v-if="editMode">
                    <input class="col-10"
                        v-model="taskTitle" 
                        style="
                            margin: 0;
                            border: inset;
                            font-family: inherit;
                            font-size: larger;
                            line-height: inherit;
                            background-color: transparent;"
                        >
                        <button 
                            class="btn btn-sm rounded-0 btn-danger " 
                            type="button" 
                            @click="delTask(task.id)" 
                            title="delete" 
                            style="color: white; ; float:right"       
                            >
                            <i class="" ></i>X</button>
                </form>
                <hr >
                        <label v-if="editMode">Move Category :</label>
                        <select
                            v-if="editMode"
                            v-model="categoryId"
                            id="category-edit"
                            style="width: auto;border: inset;"
                            aria-label="category-edit"
                            >
                            <option 
                                v-for="category in categories" 
                                :key="category.id" 
                                :value="category.id"
                                >{{category.name}}
                            </option>
                        </select>
                        <label  v-if="editMode">
                            <button 
                                class="btn btn-sm btn-success rounded-0  " 
                                type="button" 
                                @click="moveCat" 
                                title="move" 
                                style="color: white; "       
                                >
                                <i></i>Move</button>
                        </label>
                        
                    <hr v-if="editMode">
                        <label class="cal" ><small>by: {{getById(task.userId)}}</small></label>
                        <label style="float:right;">
                            <ul class="list-inline m-0" v-if="editMode ">
                                <li class="list-inline-item">
                                    <button class="btn btn-success btn-sm rounded-0" type="button" @click="editTask" title="Edit" ><i class="bi bi-pencil" ></i>Edit</button>
                                </li>
                                <li class="list-inline-item">
                                    <button class="btn btn-danger btn-sm rounded-0" type="button" title="cancel" @click="cancel"><i class="bi bi-trash" ></i>Cancel</button>
                                </li>
                            </ul>
                        </label>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import Swal from 'sweetalert2'

export default {
    name: 'TaskCard',
    props:['task', 'user', 'categories' ],
    data () {
        return {
            editMode: false,
            categoryId: 0,
            taskTitle: ''
        }
    },
    methods : {
        getById (val) {
            return this.user.filter(user => user.id === val)[0].email
        },
        editTask () {
           this.$emit('editTask', {
               id: this.task.id,
               name: this.taskTitle,
               categoryId: this.categoryId
           })
           
        },
        delTask (id) {
            Swal.fire({
                title: 'Are you sure?',
                text: "You won't be able to revert this!",
                icon: 'warning',
                showCancelButton: true,
                confirmButtonColor: '#3085d6',
                cancelButtonColor: '#d33',
                confirmButtonText: 'Yes, delete it!'
                }).then((result) => {
                if (result.isConfirmed) {
                    this.$emit('delTask', {
                    id:id})
                }
                })
            
        },
        editButton () {
            this.categoryId = this.task.categoryId
            this.taskTitle = this.task.name
            this.editMode = true
        },
        cancel () {
            this.editMode = false
        },
        moveCat () {
            this.$emit('moveCat', {
               id: this.task.id,
               categoryId: this.categoryId
           })
        }
    }
}
</script>

<style>

</style>