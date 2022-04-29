<template>
    <NavBar/>
  <div class="container-fluid text-center pt-3 ">
    <div class="row justify-content-around">
      
      <div class="col-md-7">
        <FilterProjects @filter-projects="changeProjects"/>
        <i v-if="loading" class="fa-solid fa-circle-notch fa-spin mx-3"></i>
        <div v-else-if="projects.length>0" class="projects">
          <table class="table table-hover">
            <thead class="fw-bold">
              <td>Date debut</td>
              <td>Date fin</td>
              <td>Projets</td>
              <td>Client</td>
            </thead>
            <tbody>
              <tr v-for="project in projects" :key="project._id" @click="redirectToDetails(project._id)">
                <td>{{project.date_debut}}</td>
                <td>{{project.date_fin}}</td>
                <td>{{project.nom}}</td>
                <td>{{project.client}}</td>
              </tr>
            </tbody>
          </table>
        </div>
        <div v-else>
          <h4 class="text-muted m-3">Aucun Projet</h4>
        </div>
      </div>
    </div>
    
  </div>
</template>

<script>
import EventBus from "@vertx/eventbus-bridge-client.js";
import FilterProjects from "./home-page/FilterProjects.vue";
import NavBar from "./home-page/NavBar.vue";

export default {
  name:"HomePage",
  components:{
    FilterProjects,NavBar
  },
  data(){
    return{
      projects:[],
      loading:true
    }
  },
  methods:{
    redirectToAdd(){
      this.$router.push('/ajouter')
    },
    redirectToDetails(id){
      this.$router.push(`/project/${id}`)
    },
    changeProjects({msg}){
      this.projects=msg
    }
  },
  mounted(){
    if(sessionStorage.getItem('id')===null){
      this.$router.push('/auth')
    }
    var ref=this
    var eb = new EventBus('http://localhost:8888/eventbus');
    eb.onopen = function() {
        console.log('connected');
        eb.send('get.project.all', "",(err,msg) =>{
          if(err){
            ref.loading=false
            console.log(err);
            return
          }
          ref.loading=false
          ref.projects=msg.body
        });
    };
    eb.onclose = function() {
        console.log("disconnected");
        eb = null;
    };
  }
}
</script>

<style>
  .projects{
    height: 300px;
    overflow-y: auto;
  }
  tr:hover{
    cursor: pointer;
  }
</style>