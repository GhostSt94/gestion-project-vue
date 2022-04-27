<template>
  <div class="container">
        <div class="row justify-content-center">
            <div v-if="updating && not_found" class="text-center mt-5">
                <h3 class="text-muted">CLIENT NOT FOUND</h3>
                <router-link to="/clients">Liste des clients</router-link>
            </div>
            <div v-else class="col-md-6 mt-4 py-4 px-3 rounded shadow">
                <i @click="back" class="fa-solid fa-arrow-left-long float-start text-secondary back"></i>
                <i v-show="updating" @click="deleteClient" class="fa-solid fa-trash-can text-danger float-end"></i>
                <h3 v-if="!updating" class="text-center p-2">Ajouter Client</h3>
                <h3 v-else class="text-center p-2">Modifier Client</h3>
                <div class="form-floating mb-3">
                    <input v-model="nom" type="text" class="form-control" id="floatingInput1" >
                    <label for="floatingInput1">Nom Client</label>
                </div>
                <div class="row">
                    <div class="col">
                        <div class="form-floating mb-3">
                            <input v-model="ville" type="text" class="form-control" id="floatingInput5" >
                            <label for="floatingInput5">Ville</label>
                        </div>
                    </div>
                    <div class="col">
                        <div class="form-floating mb-3">
                            <input v-model="contact" type="text" class="form-control" id="floatingInput2" >
                            <label for="floatingInput2">Contact</label>
                        </div>
                    </div>
                </div>
                <div class="form-floating mb-3">
                    <input v-model="email" type="mail" class="form-control" id="floatingInput5" >
                    <label for="floatingInput5">Mail</label>
                </div>
                <div class="row mb-3">
                    <div class="col">
                        <div class="form-floating">
                            <input v-model="responsable_client" type="text" class="form-control" id="floatingInput3">
                            <label for="floatingInput3">Responsable client</label>
                        </div>
                    </div>
                    <div class="col">
                        <div class="form-floating">
                            <input v-model="responsable_projet" type="text" class="form-control" id="floatingInput4" >
                            <label for="floatingInput4">Responsable projet</label>
                        </div>
                    </div>
                </div>  
                <div class="row mb-3">
                    <div class="col-2 text-center">
                        <label class="h6">Secteur</label>
                    </div>
                    <div class="col-4">
                        <select v-model="secteur" class="form-select form-select-md">
                            <option value="Administratif">Administratif</option>
                            <option value="Privé">Privé</option>
                        </select>
                    </div>
                </div>

                <div v-if="error!=''" class="alert alert-danger">{{error}}</div>
                <div class="float-end">
                    <i v-if="loading" class="fa-solid fa-circle-notch fa-spin mx-3"></i>
                    <button v-if="!updating" @click="addClient" class="btn btn-primary">Ajouter</button>
                    <button v-else @click="addClient" class="btn btn-warning">Modifier</button>
                </div>
            </div>
        </div>
  </div>
</template>

<script>
import axios from "axios";
import Swal from 'sweetalert2'

export default {
    data(){
        return{
            nom:"",
            contact:"",
            email:"",
            responsable_client:"",
            responsable_projet:"",
            ville:"",
            secteur:"",
            error:'',
            loading:false,
            updating:false,
            not_found:false,
            // 
            url:"http://localhost:8888/clients/"
        }
    },
    methods:{
        back(){
            this.$router.back()
        },
        addClient(){
            var client={}
            if(this.nom===""){
                this.error="Veuillez entrer le nom du client"
                return
            }else{
                client.nom=this.nom
            }
            this.contact!==""?client.contact=this.contact:null
            this.email!==""?client.email=this.email:null
            this.responsable_client!==""?client.responsable_client=this.responsable_client:null
            this.responsable_projet!==""?client.responsable_projet=this.responsable_projet:null
            this.ville!==""?client.ville=this.ville:null
            this.secteur!==""?client.secteur=this.secteur:null

            this.loading=true
            this.error=""
            if(this.updating){
                axios.put(this.url+this.$route.params.id,client)
                .then(res=>{
                    this.loading=false
                    console.log(res)
                    Swal.fire('Client Modifié')
                    this.$router.back()
                    })
                .catch(err=>{
                    this.loading=false
                    this.error=err.response.data
                    })
            }else{
                axios.post(this.url,client)
                .then(res=>{
                    this.loading=false
                    console.log(res)
                    Swal.fire('Client Ajouté')
                    this.$router.back()
                    })
                .catch(err=>{
                    this.loading=false
                    this.error=err.response.data
                    })
            }
        },
        deleteClient(){
            Swal.fire({
                title: 'Supprimer client ?',
                showCancelButton: true,
                confirmButtonText: 'confirmer',
                }).then((result) => {
                if (result.isConfirmed) {
                    axios.delete(this.url+this.$route.params.id)
                    .then(res=>{
                        console.log(res)
                        Swal.fire('Client Supprimé')
                        this.$router.back()
                    })
                    .catch(err=>{
                        this.loading=false
                        this.error=err.response.data
                    })
                }
            })
        }
    },
    mounted(){
        if(this.$route.name==="ClientDetails"){
            this.updating=true
            axios.get(this.url+this.$route.params.id)
            .then(({data})=>{
                this.nom=data.nom
                data.contact?this.contact=data.contact:null
                data.email?this.contact=data.email:null
                data.responsable_client?this.responsable_client=data.responsable_client:null
                data.responsable_projet?this.responsable_projet=data.responsable_projet:null
                data.ville?this.ville=data.ville:null
                data.secteur?this.secteur=data.secteur:null
                })
            .catch(err=>{
                this.not_found=true
                this.error=err.response.data
                })
        }
    }
}
</script>

<style >
    .back:hover{
        cursor: pointer;
        color: black !important;
    }
</style>