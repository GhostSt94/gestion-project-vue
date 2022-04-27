<template>
  <div class="container">
        <div class="position-absolute top-50 start-50 translate-middle w-25 h-50 rounded shadow-lg text-center p-3">
            <h3 class="m-2 mb-3">Register user</h3>
            <div class="form-floating mb-3">
                <input v-model="username" type="text" class="form-control" id="floatingInput1" >
                <label for="floatingInput1">Username</label>
            </div> 
            <div class="form-floating mb-3">
                <input v-model="password" type="password" class="form-control" id="floatingInput2" >
                <label for="floatingInput2">Password</label>
            </div>
            <div v-if="msg!=''" class="alert alert-danger">{{msg}}</div> 
            <button @click="register" class="btn btn-primary float-end">register</button>   
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
            url:"http://localhost:8888/auth/",
            msg:""
        }
    },
    methods:{
        register(){
            const credentiels={
                username:this.username,
                password:this.password
            }
            axios.post(this.url+"register",credentiels)
            .then(res=>{
                console.log(res.data.username);
            })
            .catch(err=>console.log(err))
            // var eb = new EventBus('http://localhost:8888/eventbus');
            // eb.onopen = function() {
            //     eb.send('user.credentials', JSON.stringify(credentiels),(err,msg) =>{
            //     if(err){
            //         console.log(err);
            //         return
            //     }
            //     alert(msg.body)
            //     });
            // };
        }
    }
}
</script>

<style>

</style>