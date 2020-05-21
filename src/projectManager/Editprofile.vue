<template>
  <div>
    <menu1></menu1>
         <div id="main-content">
            <div class="container-fluid">
                <div class="block-header">
                    <div class="row">
                        <div class="col-lg-6 col-md-8 col-sm-12">
                            <h2><a href="javascript:void(0);" class="btn btn-xs btn-link btn-toggle-fullwidth"><i
                                        class="fa fa-arrow-left"></i></a> Update Your Profile</h2>
                            <ul class="breadcrumb">
                                <li class="breadcrumb-item"><router-link to="/dashboard"><i class="icon-home"></i></router-link></li>
                                <li class="breadcrumb-item">Employee</li>
                                <li class="breadcrumb-item">profile</li>
                                <li class="breadcrumb-item active">EditProfile</li>
                            </ul>
                        </div>

                    </div>
                </div>

                <div class="row clearfix">
                    <div class="col-12">
                        <div class="card">
                            <div class="body">
                                <div style="margin-left:20%;" class="row clearfix">
                                    <div class="col-sm-8">
                                        <div class="form-group">
                                            <input type="text" style="font-size:20px;font-family:'serif'" class="form-control" placeholder="First Name *" v-model="emparr.empFirstName">
                                        </div>
                                    </div>
                                    <div class="col-sm-8">
                                        <div class="form-group">
                                            <input type="text" style="font-size:20px;font-family:'serif'" class="form-control" placeholder="Middle Name"  v-model="emparr.empMiddleName">
                                        </div>
                                    </div>
                                    <div class="col-sm-8">
                                        <div class="form-group">
                                            <input type="text" style="font-size:20px;font-family:'serif'" class="form-control" placeholder="Last Name"  v-model="emparr.empLastName">
                                        </div>
                                    </div>
                                    <div class="col-sm-8">
                                        <div class="form-group">
                                            <input type="text" style="font-size:20px;font-family:'serif'" class="form-control" placeholder="Email ID *"  v-model="emparr.empEmailId">
                                        </div>
                                    </div>
                                    <div class="col-sm-8">
                                        <div class="form-group">
                                            <input type="text" style="font-size:20px;font-family:'serif'" class="form-control" placeholder="Mobile No"  v-model="emparr.empContactNo">
                                        </div>
                                    </div>
                                </div>
                                <div class="col-sm-8">
                                <button style="width:50%;margin-left:50%;" type="button" @click="Onupdate" class="btn btn-primary">Update</button>
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
import employee from '../services/employee'
export default {
    data:function(){
      return{
        empId:'',
        emparr:{}
      }
    },
    created(){
      employee.getempbyid(localStorage.getItem('empId')).then(doc=>{
        this.emparr = doc.data[0];
        console.log(this.emparr);
      })
    },
    methods:{
      Onupdate:function(){
        employee.updateemp(this.emparr).then(doc=>{
          console.log(doc.data);
          this.$fire({
                    title: "profile successfully updated",
                    type: "success",
                    timer: 3000
                    }).then(r => {
                    console.log(r.value);
                    });
          this.$router.push("/profile");
        })
      }
    }

}
</script>
