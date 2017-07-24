<template>    
            <tr>
                <td>
                    <input type="text" class="form-control"
                        v-model="editForm.username" v-if="edit">
                    <span v-else>{{user.username}}</span>
                </td>
                <td>
                    <input type="email" class="form-control"
                        v-model="editForm.email" v-if="edit">
                    <span v-else>{{user.email}}</span>
                </td>
                <td>
                    <button v-on:click="editUser" type="button" class="btn btn-info" v-if="!edit">Edit</button>
                    <button v-on:click="updateUser(user,editForm)" type="button" class="btn btn-success" v-if="edit">Save</button>
                    <button v-on:click="cancleEdit" type="button" class="btn btn-default" v-if="edit">Cancle</button>
                    <button v-on:click="$emit('delete-user',user)" type="button" class="btn btn-danger" v-if="!edit">Delete</button>
                </td>
            </tr>
            
</template>

<script>
    export default {
        props: ['user'],
        data(){
            return{
                edit: false,
                editForm:{
                    username:'',
                    email:''
                }
            }
        },
        methods:{
            editUser(){
                this.edit=true;
                this.editForm.username = this.user.username;
                this.editForm.email = this.user.email;
            },
            cancleEdit(){
                this.edit=false;
                this.editForm.username = '';
                this.editForm.email = '';
            },
            updateUser(oldUser,newUser){
                this.$http.patch('/users/'+oldUser.id,newUser).then(response=>{
                    this.$emit('update-user');
                    this.cancleEdit();
                    console.log(response.data);
                });
            }
        }

    }
</script>
