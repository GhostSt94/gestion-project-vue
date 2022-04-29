<template>
    <div class="container pt-4">
        <i @click="back" class="fa-solid fa-arrow-left-long float-start text-secondary back m-2"></i>
        <h4 class="ms-5">Liste des Clients <i class="fa-solid fa-plus btn btn-outline-primary rounded-pill ms-1" @click="goToAddPage"></i></h4>
        <div class="row pt-3">
            <router-link :to="{ name: 'ClientDetails', params: { id: cl._id }}" v-for="cl in clients" :key="cl._id" class="col-3 m-2 p-2 bg-white rounded shadow-sm">
                {{cl.nom}}
            </router-link>
        </div>
    </div>
</template>

<script>
import axios from "axios";
export default {
    data(){
        return{
            clients:[]
        }
    },
    methods:{
        back(){
            this.$router.back()
        },
        goToAddPage(){
            this.$router.push("/ajouter/client")
        }
    },
    mounted(){
        if(sessionStorage.getItem('id')===null){
            this.$router.push('/auth')
        }
        axios.get('http://localhost:8888/clients')
        .then(res=>{this.clients=res.data})
        .catch(err=>console.log(err))
    }
}
</script>

<style scoped>
    a{
        color:grey;
    }
    a:hover{
        color: rgb(239, 107, 41);;
    }
</style>