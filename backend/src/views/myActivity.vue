<template>
<div v-if="isShow">
<el-row style="height:150px">
  <el-col :span="10" v-for="(item, index) in ActiveList" :key="index" :offset="index % 2>0?2:0" style="height:180px">
    <el-card :body-style="{ padding: '0px' }" style="height:170px;box-shadow: 0 8px 4px rgba(0, 0, 0, .12);">
      
      <div style="padding: 14px;">
        <span></span>
        <h4>{{item.POSTTITLE}}</h4>
        <br />
        <div style="word-wrap:break-word;overflow-y:auto;height:80px;font-size:4px;">
          <p>{{item.CONTENT}}</p>
        </div>
        <div>
          <el-row>
            <el-col :span="8"></el-col>
            <el-col :span="4">
                <el-button type="text" class="button" @click="openPost(item)">打开帖子</el-button>
            </el-col>
            <el-col :span="4">
              <el-popconfirm @confirm="deletePost(item.POSTID)"  title="这是一段内容确定删除吗？">
                <el-button type="text" slot="reference">删除</el-button>
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
  <p>没有发帖内容</p>
</div>
</template>

<script>
//import func from 'vue-editor-bridge'
export default {
  data()
  {
    return{
      UserID:'',
      ActiveList:[],
      url:'/postid=',
      isShow:false,
    }
  },
  mounted()
  {
    this.setUserID();
    this.getActive(this.UserID);
  },
  methods:{
    test:function()
    {
      console.log("ok");
    }, 
    setUserID()
    {
      var storeID = localStorage.getItem("userID");
      this.UserID = storeID;
    },
    storeActive(respons)
    {
      this.ActiveList = JSON.parse(respons);
      console.log(this.ActiveList);
      if(this.ActiveList.length==0)
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
      var url='http://139.196.167.75:5000/api/GetTieziB'+'/'+params;
      await fetch(url, {
          method: 'GET',
          headers: {
            'Accept': 'application/json',
            'Content-Type': 'application/json'
          },
          //body: JSON.stringify(UpdateData)
        })
        .then(respons => respons.json())
        .then(respons => this.storeActive(respons));
    },
    openPost(item)
    {
      console.log(item);
      
      this.$router.push({name:'BrowsePost',query:{postInfo:JSON.stringify(item)}});
    },

    async deletePost(POSTID)
    {
      console.log("删除贴子"+POSTID);
      var url='http://139.196.167.75:5000/api/PostTiezi/'+POSTID;
      await fetch(url, {
        method: 'DELETE',
        headers: {
          'Accept': 'application/json',
          'Content-Type': 'application/json'
        },
      });
      this.$message("删除成功");
      location.reload();

    }
  }
  
}
</script>
<style>

</style>