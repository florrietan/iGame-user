<!-- 我关注的贴吧-->
<template>
<div v-if="hasFollow">
<el-row style="height:150px">
  <el-col :span="10" v-for="(item, index) in myBarList" :key="index" :offset="index % 2>0?2:0" style="height:180px">
    <el-card :body-style="{ padding: '0px' }" style="height:170px;box-shadow: 0 4px 4px rgba(0, 0, 0, .12);">
      
      <div style="padding: 14px;">
        <span></span>
        <h4>{{item.TIEBAID}}</h4>
        <br />
        <div style="word-wrap:break-word;overflow-y:auto;height:80px;font-size:4px;">
          <p>{{item.DESCRIPTION}}</p>
        </div>
        <div>
          <el-row>
            <el-col :span="8"></el-col>
            <el-col :span="4">
                <el-button type="text" class="button" @click="openBar(item)">打开贴吧</el-button>
            </el-col>
            <el-col :span="4">
              <el-popconfirm @confirm="deleteFo(item.TIEBAID)"  title="确定取消关注吗">
                <el-button type="text" slot="reference">取消关注</el-button>
              </el-popconfirm>
            </el-col>
            <el-col :span="8"></el-col>
          </el-row>
        </div>
      </div>
    </el-card>
  </el-col>
</el-row>
</div>
<div v-else>
  <p>没有关注内容</p>
</div>



</template>

<script>

export default {
  data()
  {
    return{
      UserID:'',
      myBarList:[],
      hasFollow:false,
      
    }
  },
  mounted()
  {
    this.setUserID();
    this.getMyBar(this.UserID);
    
  },
  methods:{
    setUserID()
    {
      var storeID = localStorage.getItem("userID");
      this.UserID = storeID;
    },
    storeMyBar(respons)
    {
      this.myBarList = JSON.parse(respons);
      if(this.myBarList.length==0)
      {
        this.hasFollow=false;
      }
      else
      {
        this.hasFollow=true;
      }
      //console.log(this.myBarList);
    },
    //获得关注
    async getMyBar(params)
    {
      var url='http://139.196.167.75:5000/api/FollowTieBa'+'/'+params;
      await fetch(url, {
          method: 'GET',
          headers: {
            'Accept': 'application/json',
            'Content-Type': 'application/json'
          },
         
        })
        .then(respons => respons.json())
        .then(respons => this.storeMyBar(respons));
      
        console.log("获得关注")
    },
    //取消关注
    async deleteFo(tiebaID){
       const data={
         USERID: this.UserID,
        TIEBAID: tiebaID,
      }
            console.log(tiebaID);
            
          await fetch('http://139.196.167.75:5000/api/FollowTieBa', {
            method: 'DELETE',
            headers: {
              'Accept': 'application/json',
              'Content-Type': 'application/json'
            },
            body: JSON.stringify(data)
          });
         
          this.getMyBar(this.UserID);

        },
    openBar(item)
    {
      //console.log(TIEBAID);
     // var url=''+'/'+ TIEBAID;
      this.$router.push({name:'BarPage',query:{barInfo:JSON.stringify(item)}});
      
    }

  }
  
}
</script>

<style>

</style>