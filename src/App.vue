<template>
    <div>
        <LoginForm 
            v-if="page === 'login'"
            @login="login"
            @changePage="changePage"
            @onSignIn='onSignIn'
            >
        </LoginForm>
        <RegisterForm 
            v-if="page === 'register'"
            @changePage="changePage"
            @register='register'
            >
        </RegisterForm>
        <Kanban 
            v-if="page === 'kanban'"
            @logout="logout"
            @getKanban='getKanban'
            @getCategory='getCategory'
            @addCategory='addCategory'
            @addTask='addTask'
            @getUser='getUser'
            @changePage="changePage"
            @delTask='delTask'
            @delCategory='delCategory'
            @editTask='editTask'
            @moveCat='moveCat'
            :categories='categories'
            :tasks='tasks'
            :user='user'
        ></Kanban>
    </div>
</template>

<script>
import LoginForm from "./components/login"
import RegisterForm from "./components/register"
import Kanban from "./views/kanban"
import axios from './configs/axiosInstance'
import Swal from 'sweetalert2'

export default {
    name: "App",
    data () {
        return {
            message: 'hello world',
            page: 'kanban',
            categories: [],
            tasks: [],
            user: []
        }
    },
    components: {
        LoginForm,
        RegisterForm,
        Kanban
    },
    methods: {
        logout () {
            localStorage.clear()
            this.checkAuth()
            Swal.fire({
                position: 'center',
                icon: 'success',
                title: 'Logged Out',
                showConfirmButton: false,
                timer: 700
                })
        },
        getKanban () {
            axios({
                method: 'get',
                url: `/tasks`,
                headers: {
                    access_token: localStorage.access_token
                }
            }).then(response => {
                // console.log(response);
                this.tasks = response.data
            }).catch(err => {
                console.log(err.response.data.errors);
                Swal.fire({
                    icon: 'error',
                    title: 'Oops...',
                    text: 'Error while getting task list',
                    footer: '<a >This might be caused by bad request</a>'
                    })
            })
        },
        getCategory () {
            axios({
                method: 'get',
                url: `/category`,
                headers: {
                    access_token: localStorage.access_token
                }
            }).then(response => {
                // console.log(response);
                this.categories = response.data
                this.checkAuth()
            }).catch(err => {
                console.log(err.response.data.errors);
                Swal.fire({
                    icon: 'error',
                    title: 'Oops...',
                    text: 'Error while getting category list',
                    footer: '<a >This might be caused by bad request</a>'
                    })
            })
        },
        getUser () {
            axios({
                method: 'get',
                url: `/user`,
                
            }).then(response => {
                // console.log(response);
                this.user = response.data
                this.checkAuth()
            }).catch(err => {
                console.log(err.response.data.errors);
                Swal.fire({
                    icon: 'error',
                    title: 'Oops...',
                    text: 'Error while getting user list',
                    footer: '<a >This might be caused by bad request</a>'
                    })
            })
        },
        checkAuth () {
            if(localStorage.getItem('access_token')) {
                this.changePage('kanban')
                //get kanban list
            } else {
                this.changePage('login')
            }
        },
        changePage (page) {
            this.page = page
        },
        login (payload) {
            console.log('login');
            const { email, password } = payload
            axios({
                method: 'POST',
                url: `/login`,
                data: {
                    email: email,
                    password: password
                }
            }).then(response => {
                // console.log(response.data.access_token);
                localStorage.setItem('access_token', response.data.access_token)
                this.checkAuth() 
                Swal.fire({
                    title: 'Welcome',
                    width: 300,
                    padding: '1em',
                    background: '#cfdac8 url(https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSDigSTLAGIO6FB0y-rYmlKluOZsxjwvb6v-Q&usqp=CAU)',
                    timer: 1500,
                    backdrop: `
                        rgba(0,0,123,0.4)
                        url("https://media3.giphy.com/media/xUPGGDNsLvqsBOhuU0/giphy.gif?cid=ecf05e471zquwlkegsvr3deeh2xdf5z718ivhw3dixymm67k&rid=giphy.gif")
                        top
                        no-repeat
                    `
                })
                

            }).catch(err => {
                let errors = err.response.data.errors
                errors.forEach(element => {
                    Swal.fire({
                    icon: 'error',
                    title: 'Oops...',
                    text: `${element}`,
                    footer: '<a >please follow the rules</a>'
                    })
                });
            })
        },
        register (payload) {
            console.log('register');
            const { email, password } = payload
            axios({
                method: 'POST',
                url: `/register`,
                data: {
                    email: email,
                    password: password
                }
            }).then(response => {
                Swal.fire({
                    position: 'center',
                    icon: 'success',
                    title: 'User Created Successfully',
                    showConfirmButton: false,
                    timer: 1500
                    })
                this.checkAuth() 
            }).catch(err => {
                console.log(err.response.data.errors);
                let errors = err.response.data.errors
                errors.forEach(element => {
                    Swal.fire({
                    icon: 'error',
                    title: 'Oops...',
                    text: `${element}`,
                    footer: '<a >please follow the rules</a>'
                    })
                });
                
            })
        },
        addTask (payload) {
            const { name, categoryId } = payload
            axios({
                method: 'post',
                url: `/tasks`,
                data: {
                    name: name,
                    categoryId: categoryId
                },
                headers: {
                    access_token: localStorage.access_token
                }
            }).then(response => {
                // console.log(response);
                Swal.fire({
                    position: 'center',
                    icon: 'success',
                    title: 'Task Created',
                    showConfirmButton: false,
                    timer: 1500
                    })
            }).catch(err => {
                let errors = err.response.data.errors
                    errors.forEach(element => {
                        Swal.fire({
                        icon: 'error',
                        title: 'Oops...',
                        text: `${element}`,
                        footer: '<a >please follow the rules</a>'
                        })
                    });
                // console.log(err.response.data.errors);
            })
        },
        addCategory (payload) {
            console.log(payload);
            const { name } = payload
            axios({
                method: 'post',
                url: `/category`,
                data: {
                    name: name,
                },
                headers: {
                    access_token: localStorage.access_token
                }
            }).then(response => {
                Swal.fire({
                    position: 'center',
                    icon: 'success',
                    title: 'Category Created',
                    showConfirmButton: false,
                    timer: 1500
                    })
                // console.log(response);
            }).catch(err => {
                let errors = err.response.data.errors
                errors.forEach(element => {
                    Swal.fire({
                    icon: 'error',
                    title: 'Oops...',
                    text: `${element}`,
                    footer: '<a >please follow the rules</a>'
                    })
                });
            })

        },
        editTask (payload) {
            const { id,name,categoryId } = payload
            axios({
                method: 'put',
                url: `/tasks/${id}`,
                data: {
                    name: name,
                    categoryId: categoryId
                },
                headers: {
                    access_token: localStorage.access_token
                }
            }).then(response => {
                Swal.fire({
                    position: 'center',
                    icon: 'success',
                    title: 'Success Updating Task',
                    showConfirmButton: false,
                    timer: 1500
                    })
                // console.log(response);
                this.getKanban()
            }).catch(err => {
                let errors = err.response.data.errors
                    errors.forEach(element => {
                        Swal.fire({
                        icon: 'error',
                        title: 'Oops...',
                        text: `${element}`,
                        footer: '<a >please follow the rules</a>'
                        })
                    });
            })
        },
        delTask(payload) {
            const { id } = payload
            axios({
                method: 'delete',
                url: `/tasks/${id}`,
                headers: {
                    access_token: localStorage.access_token
                }
            }).then(response => {
                // console.log(response);
               Swal.fire({
                    position: 'center',
                    icon: 'success',
                    title: 'Task Deleted Successfully',
                    showConfirmButton: false,
                    timer: 1500
                    })
                this.getKanban()
            }).catch(err => {
                let errors = err.response.data.errors
                    errors.forEach(element => {
                        Swal.fire({
                        icon: 'error',
                        title: 'Oops...',
                        text: `${element}`,
                        footer: '<a >please follow the rules</a>'
                        })
                    });
            })
        },
        delCategory(payload) {
            const { id } = payload
            axios({
                method: 'delete',
                url: `/category/${id}`,
                headers: {
                    access_token: localStorage.access_token
                }
            }).then(response => {
                // console.log(response);
               Swal.fire({
                    position: 'center',
                    icon: 'success',
                    title: 'Category Deleted Successfully',
                    showConfirmButton: false,
                    timer: 1500
                    })
                this.getCategory()
            }).catch(err => {
                let errors = err.response.data.errors
                    errors.forEach(element => {
                        Swal.fire({
                        icon: 'error',
                        title: 'Oops...',
                        text: `${element}`,
                        footer: '<a >please follow the rules</a>'
                        })
                    });
            })
        },
        moveCat (payload) {
            const { id,categoryId } = payload
            axios({
                method: 'patch',
                url: `/tasks/${id}`,
                data: {
                    categoryId: categoryId
                },
                headers: {
                    access_token: localStorage.access_token
                }
            }).then(response => {
                // console.log(response);
                Swal.fire({
                    position: 'center',
                    icon: 'success',
                    title: 'Your task has been moved',
                    showConfirmButton: false,
                    timer: 1500
                    })
                this.getKanban()
            }).catch(err => {
                let errors = err.response.data.errors
                    errors.forEach(element => {
                        Swal.fire({
                        icon: 'error',
                        title: 'Oops...',
                        text: `${element}`,
                        footer: '<a >please follow the rules</a>'
                        })
                    });
            })
        },
        onSignIn(googleUser) {
            console.log('masuk sini');
            const profile = googleUser.getBasicProfile();
            const id_token = googleUser.getAuthResponse().id_token;
            axios({
                method: 'post',
                url: `/loginGoogle`,
                data : {
                    id_token
                }
            })
            .then(response => {
               localStorage.setItem('access_token', response.data.access_token)
               this.checkAuth() 
                Swal.fire({
                    position: 'center',
                    icon: 'success',
                    title: 'Logged In',
                    showConfirmButton: false,
                    timer: 1500
                    })
            })
            .catch(err => {
                let errors = err.response.data.errors
                    errors.forEach(element => {
                        Swal.fire({
                        icon: 'error',
                        title: 'Oops...',
                        text: `${element}`,
                        footer: '<a >please follow the rules</a>'
                        })
                    });
            })
    }

    },
    created() {
        this.checkAuth() 
    }
}
</script>

<style>
    
</style>