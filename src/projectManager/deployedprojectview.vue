<template>
<div>
  <menu1></menu1>
   <div id="main-content" class="profilepage_1">
            <div class="container-fluid">
                <div class="block-header">
                    <div class="row">
                        <div class="col-lg-6 col-md-8 col-sm-12">
                            <h2><a href="javascript:void(0);" class="btn btn-xs btn-link btn-toggle-fullwidth"><i
                                        class="fa fa-arrow-left"></i></a></h2>
                            <ul class="breadcrumb">

                            </ul>
                        </div>

                    </div>
                </div>

                <div class="row clearfix">
                    <div class="col-lg-12 col-md-12">
                        <div class="card">
                            <div class="body">
                                <h5>Deployed Project List </h5>
                                <br><br>
                                <div class="row clearfix" v-if="deployedproject.length==0">
                                    <div class="col-sm-12">
                                        <div class="card" >
                                            <div class="body text-left pro-img">
                                                <img src="../assets/images/nodeployedproject.png" style="height:350px;margin-left:20%;">
                                            </div>
                                        </div>
                                    </div>
                                </div>
                                <div class="row clearfix">
                                    <div class="col-lg-3 col-md-6 col-sm-12"  v-for="(item,id) in deployedproject" v-bind:key="id" >
                                        <div class="card border">
                                            <div class="body text-left pro-img">
                                                <img class="mx-auto d-block mb-3" src="../assets/images/projects/01.png"
                                                    alt="">
                                                <h6 class="project-title text-primary mb-3"><a
                                                        href="project-detail.html">{{item.projectName}}</a></h6>
                                                 <p>{{item.projectDescription}}</p>
                                                  <p><b>Deploy to Client :-</b> <br><span style="color:blue;">{{item.clientFirstName}} {{item.clientMiddleName}} {{item.clientLastName}}</span></p>
                                                  <p><b>Deployment Date :-</b> <br><span style="color:blue;">{{item.projectEndDate | moment("Do MMMM YYYY")}}</span></p>
                                                  <button @click="onprojectdetails(item.projectId)" class="button button1" style="margin-left:50px;">Project Detail</button>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>

                    </div>

                </div>
            </div>
        </div>
</div>
</template>

<script>
import project from '../services/project'
import emp from '../services/employee'
import employee from '../services/employee'
import moduleclass from '../services/module'
import clientclass from '../services/client'
export default {
  data:function(){
    return{
      deployedproject:[{}],
      cnt:0,
    }
  },
  created(){

    project.getprojectBystatus("Deployed").then(doc=>{
              this.deployedproject=doc.data;
              console.log(this.deployedproject);
              this.searched = doc.data;
    })

    this.deployedproject.splice(0,1);

  },
  methods:{
    onprojectdetails:function(id){
              this.$router.push({path:'/projectdetails/' + id});
    },
  },


}
</script>

<style scoped>
.button {
  background-color: #4CAF50; /* Green */
  border: none;
  color: white;
  padding: 3px 4px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 12px;
  margin: 10px 1px 10px 30px;
  transition-duration: 0.4s;
  cursor: pointer;
}

.button1 {
  background-color: white;
  color: black;
  border: 2px solid #4CAF50;
}
.button1:hover {
    background-color: #4CAF50;
    color: white;
  }

</style>
