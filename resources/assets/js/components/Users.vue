<template>
    <div>
    <form v-on:submit.prevent="createUser" method="post" >
        <div v-bind:class="{'form-group':true, 'has-error':errors.username}">
            <label>UserName</label>
            <input type="text" v-model="user.username" class="form-control">
            <span class="help-block" v-for="error in errors.username">{{error}}</span>
        </div>
        <div v-bind:class="{'form-group':true, 'has-error':errors.email}">
            <label>Email</label>
            <input type="email" v-model="user.email" class="form-control">
            <span class="help-block" v-for="error in errors.email">{{error}}</span>
        </div>
        <div class="form-group">
            <button type="submit" class="btn btn-primary">Create User</button>
        </div>
    </form>
    <table class="table table-striped">
        <thead>
            <tr>
                <th>name</th>
                <th>email</th>
                <th>Action</th>
            </tr>
        </thead>
        <tbody>
            <User v-for="user in users" v-bind:user="user"
                v-on:delete-user="deleteUser"
                v-on:update-user="fetchUsers"></User>
        </tbody>
    </table>  
    </div>  
</template>

<script>
    import User from './User.vue';
    export default {
        data(){
            return {
                users: [],
                errors:[],
                user:{
                    username: '',
                    email: ''
                }
            }
        },
        components:{
            User
        },
        created(){
            this.fetchUsers();
        },
        methods:{
            fetchUsers(){
                this.$http.get('/users').then(response=>{
                    this.users = response.data.users;
                });
            },
            createUser(){
                this.$http.post('/users/',this.user).then(response=>{
                    this.users.push(response.data.user);
                    this.user = {username: '', email: ''};
                    if(this.errors){
                        this.errors=[];
                    }
                    console.log(response.data);
                },response=>{
                    this.errors = response.data;
                });
            },
            deleteUser(user){
                this.$http.delete('/users/'+ user.id).then(response=>{
                    let index = this.users.indexOf(user);
                    this.users.splice(index,1);
                    console.log(response.data);
                });
            }
        },
        mounted() {
            console.log('Component mounted.')
        }
    }
</script>
