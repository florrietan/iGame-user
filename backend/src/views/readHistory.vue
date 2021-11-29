<template>
<div v-if="isShow">
<el-row style="height:150px">
  <el-col :span="10" v-for="(item, index) in historyList" :key="index" :offset="index % 2>0?2:0" style="height:180px">
    <el-card :body-style="{ padding: '0px' }" style="height:170px">
      
      <div style="padding: 14px;">
        <span></span>
        <h4>{{item.POSTTITLE}}</h4>
        <br />
        <div style="word-wrap:break-word;overflow-y:auto;height:80px;font-size:4px;">
          <p>{{item.CONTENT}}</p>
        </div>
        <div>
          <el-button type="text" class="button" @click="openPost(item)">打开帖子</el-button>
        </div>
      </div>
    </el-card>
  </el-col>
</el-row>
</div>
<div v-else>
  <p>没有浏览内容</p>
</div>
</template>

<script>
//import func from 'vue-editor-bridge'
export default {
  data()
  {
    return{
      UserID:"",
      historyList:[],
      isShow:false,
    }
  },
  mounted()
  {
    this.setUserID();
    this.getActive(this.UserID);
    console.log(this.historyList)
  },
  methods:{
    setUserID()
    {
      var storeID = localStorage.getItem("userID");
      this.UserID = storeID;
    },
    storeHistoryList(respons)
    {
      this.historyList = JSON.parse(respons);
      console.log(this.historyList);
      if(this.historyList.length==0)
      {
        this.isShow=false;
      }
      else
      {
        this.isShow=true;
      }
    },
    async getActive(params)
    {
      var url='http://139.196.167.75:5000/api/History'+'/'+params;
      await fetch(url, {
          method: 'GET',
          headers: {
            'Accept': 'application/json',
            'Content-Type': 'application/json'
          },
          //body: JSON.stringify(UpdateData)
        })
        .then(respons => respons.json())
        .then(respons => this.storeHistoryList(respons));
    },
    openPost(item)
    {
      console.log(item);
      
      this.$router.push({name:'BrowsePost',query:{postInfo:JSON.stringify(item)}});
    },
  }
  
}
</script>