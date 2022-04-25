<template>
  <div class="container">
      <div class="row p-3 mb-3 justify-content-center">
            <div class="col-md-3 col-sm-5 p-2">
                <select v-model="client" class="form-select form-select-md">
                    <option value=""></option>
                    <option v-for="cl in clients" :key="cl" :value="cl">{{cl}}</option>
                </select>
            </div>
            <div class="col-md-3 col-sm-5 p-2">
                <select v-model="status" class="form-select form-select-md">
                    <option value=""></option>
                    <option value="Prospecter">Prospecter</option>
                    <option value="En cours">En cours</option>
                    <option value="Reception provisoir">Reception provisoir</option>
                    <option value="Reception définitif">Reception définitif</option>
                    <option value="Cloturer">Cloturer</option>
                </select>
            </div>
            <div class="col-md-4 col-sm-6 p-2">
                <button @click="filterProjects" class="btn btn-secondary">Filter</button>
            </div>
      </div>
  </div>
</template>

<script>
import EventBus from "@vertx/eventbus-bridge-client.js";
export default {
    data(){
        return{
            status:"",
            client:"",
            clients:[],
        }
    },
    methods:{
        filterProjects(){
            var ref=this        
            var eb = new EventBus('http://localhost:8888/eventbus');
            eb.onopen = function() {
                if(ref.status===""&&ref.client===""){
                    eb.send('get.project.all', "",(err,msg) =>{
                    if(err){
                        ref.$emit("filter-projects",{msg:[]})
                        return
                    }
                    ref.$emit("filter-projects",{msg:msg.body})
                });
                }else{
                    var query={}
                    ref.status!=="" ? query.status=ref.status : null
                    ref.client!=="" ? query.client=ref.client :null

                    eb.send('get.project.query', JSON.stringify(query),(err,msg) =>{
                    if(err){
                        ref.$emit("filter-projects",{msg:[]})
                        return
                    }
                    ref.$emit("filter-projects",{msg:msg.body})
                    });
                }
                
            }
        },
        resetProjects(){
            var ref=this
            var eb = new EventBus('http://localhost:8888/eventbus');
            eb.onopen = function() {
                eb.send('get.project.all', "",(err,msg) =>{
                    if(err){
                        ref.$emit("filter-projects",{msg:[]})
                        return
                    }
                    ref.$emit("filter-projects",{msg:msg.body})
                });
            }
        }
    },
    mounted(){
        var ref=this
        var eb = new EventBus('http://localhost:8888/eventbus');
        eb.onopen = function() {
            eb.send('get.client.all', "",(err,msg) =>{
            if(err){
                console.log(err);
                return
            }
            ref.clients=msg.body.clients
            });
        };
    }
}
</script>

<style>
u:hover{
    cursor: pointer;
}
</style>