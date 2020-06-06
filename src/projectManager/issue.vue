<template>
  <div>
    <menu1></menu1>
    <div id="main-content" class="taskboard">
        <div class="container-fluid">
            <div class="block-header">
                <div class="row">
                    <div class="col-lg-6 col-md-8 col-sm-12">
                        <h2>Issues</h2>
                        <ul class="breadcrumb">
                            <li class="breadcrumb-item"><router-link to="/dashboard"><i class="icon-home"></i></router-link></li>
                            <li class="breadcrumb-item active">Issues</li>
                        </ul>
                    </div>
                </div>
            </div>
              <div class="row clearfix" v-if="getTotalissue==0">
                            <div class="col-sm-12">
                                        <div class="card" >
                                            <div class="body text-left pro-img">
                                                <img src="../assets/images/nodeployedproject.png" style="height:350px;margin-left:20%;">
                                            </div>
                                        </div>
                                    </div>
               </div>
                <div v-else class="row clearfix">
                <div class="col-md-12">
                    <div class="card">
                        <div class="body text-center">
                            <div id="chart" style="margin-left:30%;">
                            <apexchart type="pie" width="450" :options="chartOptions" :series="series"></apexchart>
                            </div>
                            <div style="margin-left:-6%;">
                            <h1 style="font-family: fantasy;">{{getTotalissue}}</h1>
                            <span>Total Issues</span>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="col-md-6">
                    <div class="card">
                        <div class="body text-center">
                            <div id="chart">
<apexchart type="radialBar" height="350" :options="chartOptions1" :series="series1"></apexchart>
<h3 class="m-b-0 m-t-10" style="font-family: fantasy;">{{getTotalresolveissue}}</h3>
                            <span>Resolve</span>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="col-md-6">
                    <div class="card">
                        <div class="body text-center">
                            <div id="chart1">
<apexchart type="radialBar" height="350" :options="chartOptions2" :series="series2"></apexchart>
                        <h3 class="m-b-0 m-t-10" style="font-family: fantasy;">{{getTotalpendingissue}}</h3>
                            <span>Pending</span>
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
import issueclass from '../services/issue'
export default {
    data:function(){
    return{
      projectId:'',
      getTotalissue:0,
      getTotalpendingissue:0,
      getTotalresolveissue:0,
      pendingPER:0,
      resolvePER:0,
      series: [],
          chartOptions: {
            chart: {
              width: 380,
              type: 'pie',
            },
            labels: ['Resolve', 'Pending'],
            responsive: [{
              breakpoint: 480,
              options: {
                chart: {
                  width: 200
                },
                legend: {
                  position: 'bottom'
                }
              }
            }]
          },

          series1: [],
          series2:[],
          series3:[],
          chartOptions1: {
            chart: {
              height: 350,
              type: 'radialBar',
            },
            plotOptions: {
              radialBar: {
                hollow: {
                  size: '50%',
                }
              },
            },
            labels: ['Resolved'],
          },
          chartOptions2: {
            chart: {
              height: 350,
              type: 'radialBar',
            },
            plotOptions: {
              radialBar: {
                hollow: {
                  size: '50%',
                }
              },
            },
            labels: ['Pending'],
          },
      }
  },
  mounted(){
    this.projectId=this.$route.params.projectId;
    if(!this.$route.params.projectId){
    issueclass.gettotalissueCOUNT().then(doc=>{
      this.getTotalissue=doc.data[0].cnt;

    issueclass.getTotalIssueCOUNTbystatus('resolved').then(doc1=>{
        this.getTotalresolveissue = 0;
         this.resolvePER = 0;
        this.getTotalresolveissue=doc1.data[0].cnt;
        this.series.push(doc1.data[0].cnt);
        this.resolvePER=(this.getTotalresolveissue*100)/this.getTotalissue;
        this.resolvePER = this.resolvePER.toFixed(2);
        this.series1.push(this.resolvePER);
    })
    issueclass.getTotalIssueCOUNTbystatus('pending').then(doc2=>{
        this.getTotalpendingissue = 0;
        this.pendingPER = 0;
        this.getTotalpendingissue=doc2.data[0].cnt;
        this.series.push(doc2.data[0].cnt);
        this.pendingPER=(this.getTotalpendingissue*100)/this.getTotalissue;
        this.pendingPER = this.pendingPER.toFixed(2);
        this.series2.push(this.pendingPER);
    })
    })

  }
  else{
   issueclass.gettotalprojectissueCOUNT(this.projectId).then(docc=>{
      this.getTotalissue=docc.data[0].cnt;

    issueclass.getTotalprojectIssueCOUNTbystatus(this.projectId,'resolved').then(docc1=>{
        this.getTotalresolveissue = 0;
        this.resolvePER = 0;
        this.getTotalresolveissue=docc1.data[0].cnt;
        this.series.push(this.getTotalresolveissue);
        this.resolvePER=(this.getTotalresolveissue*100)/this.getTotalissue;
        this.resolvePER = this.resolvePER.toFixed(2);
        this.series1.push(this.resolvePER);
    })
    issueclass.getTotalprojectIssueCOUNTbystatus(this.projectId,'pending').then(docc2=>{
        this.getTotalpendingissue = 0;
        this.pendingPER = 0;
        this.getTotalpendingissue=docc2.data[0].cnt;
        this.series.push(this.getTotalpendingissue);
        this.pendingPER=(this.getTotalpendingissue*100)/this.getTotalissue;
        this.pendingPER = this.pendingPER.toFixed(2);
        this.series2.push(this.pendingPER);
    })
    })
    }
  },
  methods:{

  },
}
</script>

<style scoped>
.taskboard1
{
    margin-left:100px;
}
</style>
