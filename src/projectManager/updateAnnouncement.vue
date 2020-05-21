<template>
<div>
  <menu1></menu1>
        <div id="main-content">
            <div class="container-fluid">
                <div class="block-header">
                    <div class="row">
                        <div class="col-lg-6 col-md-8 col-sm-12">
                            <h2><a href="javascript:void(0);" class="btn btn-xs btn-link btn-toggle-fullwidth"><i
                                        class="fa fa-arrow-left"></i></a> Update Announcement</h2>
                            <ul class="breadcrumb">
                                <li class="breadcrumb-item"><router-link to="/dashboard"><i class="icon-home"></i></router-link></li>
                                <li class="breadcrumb-item">Announcement</li>
                                <li class="breadcrumb-item active">Update Announcement</li>
                            </ul>
                        </div>

                    </div>
                </div>

                <div class="row clearfix">

                    <div class="col-12">

                        <div class="card">
                            <div class="body">
                                <div class="row clearfix">
                                    <div class="col-md-4 col-sm-12">
                                        <div class="form-group">
                                            <textarea cols="100" rows="3"  placeholder="Announcement Title" v-model="ann.announcementTitle"></textarea>
                                        </div>
                                        <div class="form-group">
                                            <textarea cols="100" rows="10"  placeholder="Write Your Announcements Here !!! *" v-model="ann.announcementMessage"></textarea>
                                        </div>
                                    </div>
                                </div>
                                <p style="color:green;" v-if="updateflag">Update Successfully</p>

                                <button type="button" @click="onupdate" class="btn btn-primary">UPDATE ANNOUNCEMENT</button>
                                <button type="button" @click="onback" class="btn btn-primary">BACK</button>

                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>




    </div>

</template>

<script>
import anceclass from '../services/Announcement'
export default {
  data:function(){
    return{
      updateflag:false,
      ann:{
        announcementId:'',
        announcementTitle:"",
        announcementMessage:"",
      }
    }
  },
  methods:{
    onupdate:function(){
        anceclass.updateannouncement(this.ann).then(doc=>{
            this.updateflag = true;
        })
    },
    onback:function(){
      this.$router.push({path:"/announcementview/"});
    }
  },
  created(){
      this.ann.announcementId=this.$route.params.announcementId;
      anceclass.getannouncementbyid(this.ann.announcementId).then(doc=>{
          console.log(doc);
          this.ann.announcementTitle=doc.data[0].announcementTitle;
          this.ann.announcementMessage=doc.data[0].announcementMessage;
      })
  }
}
</script>

<style scoped>

</style>
