<template>
  <div class="container">
        <div class="row justify-content-center">
            <div class="col-lg-4 rounded shadow-lg text-center mt-5 pb-2">
                <div class="row justify-content-between mb-3">
                    <div @click="toggleLogin(true)" :class="login ?'col-6 p-2 rounded auth active':'col-6 p-2 rounded auth'">
                        <h6>Login</h6>
                    </div>
                    <div @click="toggleLogin(false)" :class="login ?'col-6 p-2 rounded auth':'col-6 p-2 rounded auth active'">
                        <h6>Register</h6>
                    </div>
                </div>
                <div v-if="login" class="row p-2">
                    <div class="form-floating mb-3">
                        <input v-model="username" type="text" class="form-control" id="floatingInput1" >
                    <label for="floatingInput1">Username</label>
                    </div> 
                    <div class="form-floating mb-3">
                        <input v-model="password" type="password" class="form-control" id="floatingInput2" >
                        <label for="floatingInput2">Password</label>
                    </div>
                </div>

                <div v-else class="row p-2">
                    <div class="form-floating mb-3">
                        <input v-model="username" type="text" class="form-control" id="floatingInput1" >
                    <label for="floatingInput1">Username</label>
                    </div> 
                    <div class="form-floating mb-3">
                        <input v-model="password" type="password" class="form-control" id="floatingInput2" >
                        <label for="floatingInput2">Password</label>
                    </div>
                    <div class="form-floating mb-3">
                        <input v-model="confirm_password" type="password" class="form-control" id="floatingInput3" >
                        <label for="floatingInput3">Confirm Password</label>
                    </div>
                </div>
                
                <div v-if="err!=''" class="alert alert-danger">{{err}}</div>
                <div v-if="msg!=''" class="alert alert-success">{{msg}}</div> 

                <button v-if="!login" @click="registerUser" class="btn btn-warning float-end">register</button>   
                <button v-else @click="loginUser" class="btn btn-primary float-end">login</button> 
            </div>
        </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
    data(){
        return{
            username:"",
            password:"",
            confirm_password:"",
            url:"http://localhost:8888/auth/",
            msg:"",
            err:"",
            login:true
        }
    },
    methods:{
        registerUser(){
            if(this.username===""||this.password===""||this.confirm_password===""){
                this.err="Veuillez remplir tous les champs"
                return
            }
            if(this.password!==this.confirm_password){
                this.err="Password doesn't match"
                return
            }
            const credentiels={
                username:this.username,
                password:this.password
            }
            axios.post(this.url+"register",credentiels)
            .then(res=>{
                console.log(res.data);
                this.err=""
                this.msg="registered successfully"
            })
            .catch(err=>{
                this.msg=""
                this.err=err.response.data
                })
        },
        loginUser(){
            if(this.username===""||this.password===""){
                this.err="Veuillez remplir tous les champs"
                return
            }
            const credentiels={
                username:this.username,
                password:this.password
            }
            axios.post(this.url+"login",credentiels)
            .then(res=>{
                console.log(res.data);
                this.err=""
                this.msg="Login successfully"
                sessionStorage.setItem("id",res.data._id)
                sessionStorage.setItem("nom",res.data.username)
            })
            .catch(err=>{
                this.msg=""
                this.err=err.response.data
                })
        },
        toggleLogin(bool){
            this.msg=""
            this.err=""
            this.login=bool
        }
    }
}
</script>

<style scoped>
    .auth:hover{
        background: rgb(239, 107, 41);
        cursor: pointer;
    }
    .active{
        background: rgb(239, 107, 41);
        color: white;
    }
    label{
        margin-left: 3px;
    }
</style>