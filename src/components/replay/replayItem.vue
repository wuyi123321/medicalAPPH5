<template>
  <div class="replayItem" v-title data-title="回访管理">
  <yd-layout>
    <yd-navbar slot="navbar" title="回访表详情" bgcolor="#1D98DE" color="#fff" height="0.9rem" fontsize="0.36rem">
       <span slot="left" @click="back">
        <yd-navbar-back-icon color="#fff"></yd-navbar-back-icon>
      </span>
    </yd-navbar>
     <div class="questions">
       <div v-for="item,n in list">
          <div class="quesItem"  v-if="item.questionType==1">
            <div class="questitle">{{n+1}}.{{item.questionContent}}</div>
            <div class="questcontent">

              <yd-checkbox-group v-model="item.answer" >
                <span  v-for="city in item.questionOption">
                  <yd-checkbox  :val="city" :key="city" disabled></yd-checkbox>
                </span>
              </yd-checkbox-group>
            </div>
          </div>
         <div class="quesItem"  v-if="item.questionType==2">
           <div class="questitle">{{n+1}}.{{item.questionContent}}</div>
           <div class="questcontent">

             <yd-checkbox-group v-model="item.answer">
                <span  v-for="city in item.questionOption">
                  <yd-checkbox  :val="city" :key="city"></yd-checkbox>
                </span>
             </yd-checkbox-group>
           </div>
         </div>
         <div class="quesItem"  v-if="item.questionType==3">
           <div class="questitle">{{n+1}}.{{item.questionContent}}</div>
           <div class="questcontent">
             <yd-textarea v-model="item.answer" :show-counter="false" slot="right" placeholder="请输入内容" maxlength="100"></yd-textarea>
           </div>
         </div>
       </div>

     </div>
    <div slot="tabbar" class="send" @click="send">
      立即发送
    </div>

  </yd-layout>
  </div>
</template>


<script>
export default {
  data () {
    return {
//      token:"9f2c44de79443ada945bce9e6c0383cd",
      replayItem:{},
      list:[]
    }
  },
  created:function () {
    this.token = this.$route.query.token;
    this.patientId = this.$route.query.patientId;
    this.patientRecordId =this.$route.query.patientRecordId;
    this.replayItem = JSON.parse(localStorage.getItem("MicreplayItem"));
    console.log(this.replayItem);
      for(var i=0;i<this.replayItem.questions.length;i++){
       this.replayItem.questions[i].answer;
       if(this.replayItem.questions[i].questionOption){
        this.replayItem.questions[i].questionOption=this.replayItem.questions[i].questionOption.split(';')
      }
    }
    this.list =  this.replayItem.questions;

    console.log(this.replayItem);
  },
  mounted: function(){

  },
  watch:{

  },
  methods:{
    send:function () {
      var vm = this;
      $.ajax({
        url: vm.path+"patientManage/saveVisitRecord",
        dataType: "json",
        data:{
          token:vm.token,
          patientId:vm.patientId,
          patientRecordId:vm.patientRecordId,
          visitTableId:vm.replayItem.tableId,
          visitDate:vm.dateFns.format(new Date(),"YYYY-MM-DD hh:mm:ss")
        },
        type: "get",
        success: function(data) {
          console.log(data);
          if(data.statusCode == 0){
            window.android.jsSendMsg("回访表","http://appweb.sunwoda.com/medicalH5/index.html#/replayItemAnswer?patientRecordId="+data.dataInfo.singleData.patientRecordId);
          }
        },
        error: function() {

        }
      });

    },
    back:function () {
      this.$router.back()
    },

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .replayItem{
    width: 100%;
    height: 100%;
  }
  .questions{
    width: 100%;
    height: 100%;
    background-color: #fff;
  }
.el-checkbox+.el-checkbox{
  margin: 0;
  display: block;
  padding-bottom: 8px;
}
.el-checkbox{
  padding-bottom: 8px;
}
  .questions{
    padding-top: 8px;
    padding-left: 0.25rem;
    padding-right: 0.25rem;
  }
  .questitle{
    padding-bottom: 8px;
    color: #555;
  }
  .send{
    height: 0.9rem;
    font-size: 0.3rem;
    color: #fff;
    text-align: center;
    line-height: 0.9rem;
    background-color: #1D98DE;
  }
</style>
