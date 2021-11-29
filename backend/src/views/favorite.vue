<template>
<div v-if="hasFav">
<el-row style="height:150px">
  <el-col :span="10" v-for="(item, index) in favoriteList" :key="index" :offset="index % 2>0?2:0" style="height:180px">
    <el-card :body-style="{ padding: '0px' }" style="height:170px">
      
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
              <el-popconfirm @confirm="deleteFa(item.POSTID)"  title="这是一段内容确定删除吗？">
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
  <p>没有收藏内容</p>
</div>
</template>

<script>
export default {
    data()
  {
    return{
      UserID:'',
      favoriteList:[],
      url:'/postid=',
      hasFav:false,
    }
  },
  mounted()
  {
    this.setUserID();
    this.getFavorite(this.UserID);
  },
  methods:{
    setUserID()
    {
      var storeID = localStorage.getItem("userID");
      this.UserID = storeID;
    },
    storeFavorite(respons)
    {
      this.favoriteList = JSON.parse(respons);
      console.log(this.favoriteList);
      console.log("获得收藏")
      if(this.favoriteList.length==0)
      {
        this.hasFav=false;
      }
      else
      {
        this.hasFav=true;
      }
    },

    async getFavorite(params)
    {
      var url='http://139.196.167.75:5000/api/GetStar'+'/'+params;
      await fetch(url, {
          method: 'GET',
          headers: {
            'Accept': 'application/json',
            'Content-Type': 'application/json'
          },
        })
        .then(respons => respons.json())
        .then(respons => this.storeFavorite(respons));
    },
    //取消收藏
    async deleteFa(postID){
       const data={
         USERID: this.UserID,
        POSTID: postID,
      }
          await fetch('http://139.196.167.75:5000/api/GetStar', {
            method: 'DELETE',
            headers: {
              'Accept': 'application/json',
              'Content-Type': 'application/json'
            },
            body: JSON.stringify(data)
          });
         
          this.getFavorite(this.UserID);

        },
    openPost(item)
    {
      console.log("item is:");
      console.log(item);
      
      this.$router.push({name:'BrowsePost',query:{postInfo:JSON.stringify(item)}});


      /*console.log(POSTID);
      this.url = this.url + POSTID;
      localStorage.setItem('postID',POSTID);
      this.$router.push({path:this.url});
     
      location.reload();*/
    },
  }
  
}
</script>