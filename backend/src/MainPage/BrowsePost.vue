<template>
  <div class="BrowsePost" style="margin:auto;text-align:center; background-image: url(https://static.zhihu.com/heifetz/assets/sign_bg.db29b0fb.png); background-repeat:repeat-y;">

    <div class="line"></div>

    <div class="main-body" style="margin:2% auto; background-color: transparent; height:98.5%; width:90%;">
        <el-container style="height:90%;">
          <el-container class="post-main-column" style="width=90%;margin:0 2%;">
            <el-main style="background-color:rgba(255, 255, 255, 0.4);text-align:center;">

              <!--楼主-->
              <div class="postOwner" v-for="(item,index) in getPostItem" :key="index" :offset="index" :title = "item">
                <div class="postTitle" style="text-align:left;height:50px;font-size:40px;font-weight:600;margin-bottom:35px;">
                  <span style="background-color:transparent;display:inline-block;margin-right:25px;">「{{item.POSTTITLE}}」<br> </span>
                  <span v-if="!ifStar" style="display:inline-block;">                  
                    <el-button type="primary" style="float:center;margin:10px 0;" @click="starPost()">收藏本贴</el-button>

                  </span>
                  <span v-if="ifStar" style="display:inline-block;">
                    <el-button type="primary" style="float:center;margin:10px 0;" @click="deleteFa()">取消收藏</el-button>
                  </span>

                </div>
                <div class="comment owner"  :getPostItem="getPostItem">
                  <el-aside class="user-side">
                    <div>                
                      <ul>
                        <li>
                          <div>
                            <a target="_blank" href="/#/UserPage">
                              <img class="avatar" src="https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fup.enterdesk.com%2Fedpic%2Fc8%2Fdd%2Fb9%2Fc8ddb934a69d90216f1b406cf3975475.jpg&refer=http%3A%2F%2Fup.enterdesk.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1628668047&t=1ffbd058e5f38c29f8a9757421ff7503">
                            </a>
                          </div>
                        </li>
                        <br>

                        <li class="userName">
                        <a href="/#/UserPage" target="_blank">
                          {{item.NICKNAME}}
                        </a>
                        </li>

                        <br>
                        <li class="userSig" style="display:block;padding:0 30px;font-size:15px;">
                          <div class="signature">
                            {{item.SIGNATURE}}
                          </div>
                        </li>
                      </ul>
                    </div>
                  </el-aside>

                  <el-main class="content-side" >
                    <br><br><br><br>
                    {{item.CONTENT}}
                    <br><br><br><br>
                    <br><br><br><br>                                  
                  
                  </el-main>
                </div>
              </div>

              <!--<div class="spanAllComment" v-for="(item, index) in getItems" :key="item" :offset="index" :getContent = "getContent">-->
              <div class="spanAllComment" v-for="(item, index) in getCommentItems" :key="index+1" :offset="index" :title="item">
                <div class="comment">
                  <el-aside class="user-side">
                    <div>                
                      <ul>
                        <li>
                          <div>
                            <a style="" target="_blank" href="/#/UserPage">
                              <img class="avatar" src="https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fup.enterdesk.com%2Fedpic%2Fc8%2Fdd%2Fb9%2Fc8ddb934a69d90216f1b406cf3975475.jpg&refer=http%3A%2F%2Fup.enterdesk.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1628668047&t=1ffbd058e5f38c29f8a9757421ff7503">
                            </a>
                          </div>
                        </li>
                        <br>

                        <li class="userName">
                        <a href="/#/UserPage" target="_blank">
                          {{item.NICKNAME}}
                        </a>
                        </li>

                        <br>
                        <li class="userSig" style="display:block;">
                          <div class="signature">
                            {{item.SIGNATURE}}
                          </div>
                        </li>
                      </ul>
                    </div>
                  </el-aside>
                  <el-main class="content-side">
                                    <br><br><br><br>
                                    <br><br><br><br>
                                    {{item.COMCONTENT}}
                                    <br><br><br><br>
                                    <br><br><br><br>
                  </el-main>
                </div>
              </div> <!--end of spanAllComment-->

              <div class="postNewComment comment" style="height:auto;width:90%;padding:2% 3%;background-color:rgba(255,255,255,0.8);">
                <p style="text-align:left;font-size:20px;font-weight:500;">发表新的评论：</p><br>
                <div>
                  <el-input
                    type="textarea"
                    :autosize = "{ minRows: 6}"
                    placeholder="请输入评论内容"
                    v-model="commentContent">
                  </el-input>
                </div>

                <div>
                  <br>
                  <el-button type="primary" style="float:center;" @click="submitComment()">发表</el-button>
                </div>
              </div>
            </el-main>
          </el-container>

          <el-aside class="global-side-bar" style="border:black bold sold;background-color:transparent;width=10%;margin:0;margin-top:50px;">
            <div class="side-block post-recommend"  style="padding:5px;margin-top:50px;width:95%; height:auto; background-color:rgba(255, 255, 255, 0.9);">
              <div style="color:#2581d6;font-size:25px;height:50px;font-weight:900;">
                本吧热贴
              </div>
                <el-table :data="getHotPosts" stripe height="340px" style="width: 100%;">
                  <el-table-column
                    prop="POSTTITLE"
                    label="贴子名称"
                    width="160">
                  </el-table-column>
                  <el-table-column
                    prop="TAGNAME"
                    label="标签"
                    width="120">
                  </el-table-column>
                </el-table>
            </div>

            <div class="side-block hot-bar"  style="width:91%;padding:10px 10px; margin-top:100px;height:auto; background-color:rgba(255, 255, 255, 0.9);">
              <div style="color:#2581d6;font-size:25px;height:50px;font-weight:900;">
                贴吧热榜
              </div>
              <el-table :data="AdRankList" stripe height="550px" style="width: 100%;">
                <el-table-column
                  type="index"
                  label="排行"
                  width="80">
                </el-table-column>
                <el-table-column
                  prop="TIEBAID"
                  label="贴吧"
                  width="180">
                </el-table-column>
              </el-table>               
            </div>
          </el-aside>
        </el-container>

  
    </div>

  </div>

</template>

<script>
// import axios from 'axios'
export default {
  //name: "login",

  data() {
    return {    
      ifFirst: true,
      getPostItem: [],//
      getCommentItems: [],
      getHotPosts:[],
      commentContent:'',
      favoriteList:[],
      AdRankList:[],
      ifStar:false,
      userID:'',
      url: "http://139.196.167.75:5000/api/PostTiezi",

      isLogin:false,
      postInfo:{},
    };
  },

    mounted() {
    this.getParams();
    if(localStorage.userID != null) this.addHistory();
    this.getPostBrowser(this.postInfo.POSTID,this.postInfo.TIEBAID);
    this.userID = localStorage.userID;
    this.getFavorite(this.userID);
    this.getAdRank('http://139.196.167.75:5000/api/Ranking');
    //console.log('jh');
    //console.log(this.ifStar);
    //console.log(this.favoriteList);
  },

  methods: {
     getParams()
      {
        this.postInfo=this.$route.query.postInfo;
        this.postInfo=JSON.parse(this.postInfo);
      //  console.log("postinfo is:"+this.postInfo)
      },
      



    async deleteFa() {
       const data={
        USERID: localStorage.userID,
        POSTID: this.postInfo.POSTID,
      }
          await fetch('http://139.196.167.75:5000/api/GetStar', {
            method: 'DELETE',
            headers: {
              'Accept': 'application/json',
              'Content-Type': 'application/json'
            },
            body: JSON.stringify(data)
          });

      this.getFavorite(localStorage.userID);
      this.ifStar = false;            
    //  console.log("exe delete");
   //   console.log(this.ifStar);
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

    storeFavorite(respons)
    {
      this.favoriteList = JSON.parse(respons);
      //console.log(this.favoriteList);
      //console.log("浏览界面获得收藏 above");
      for (var i = 0; i < this.favoriteList.length; i++) {  //遍历数组

        if (this.favoriteList[i].POSTID == this.postInfo.POSTID) {  //如果为数字，则返回该元素的值
          this.ifStar = true;
          //console.log("exe");
        }
        //console.log(this.favoriteList[i].POSTID);
        //console.log(postInfo.POSTID);
        //console.log("ifStar is:");
        //console.log(this.ifStar);
      }
    },

    store1:function(item) {
        this.getPostItem=JSON.parse(item);
        //console.log('getPostItem是:');
        //console.log(this.getPostItem);

      //this.getCommentItems=JSON.parse(item2);

    },
    store2:function(item) {
        this.getCommentItems=JSON.parse(item);
        //console.log('getCommentItems是:');
        //console.log(this.getCommentItems);
    },

    store3:function(item) {
      this.getHotPosts=JSON.parse(item);
      console.log(this.getHotPosts);
    },

    rankStore(item)
    {
      this.AdRankList=JSON.parse(item);
     // console.log(this.AdRankList);
    },

    async getAdRank(url) {
      await fetch(url, {
            method: 'GET',
            headers: {
              'Accept': 'application/json',
              'Content-Type': 'application/json'
            },
          })
          .then(response => response.json())
          .then(data => this.rankStore(data));

    },
    

    async getPostBrowser(postID, tiebaID) {
      this.url = this.url + '/' + postID;
      //console.log("url是:" + this.url);

      await fetch(this.url, {//GET贴子相关信息
        method:'GET',
        headers: {
          'Accept': 'application/json',
          'Content-Type': 'application/json'
        },
      })      
      .then(response => response.json())
      .then(this.ifFirst == false)
      .then(data => this.store1(data));
      //console.log("get到了");

      await fetch("http://139.196.167.75:5000/api/GetTieziA/" + postID, {//GET所有评论
        method: 'GET',
        headers: {
          'Accept': 'application/json',
          'Content-Type': 'application/json'
        },
      })
      .then(response => response.json())
      .then(data => this.store2(data));

      await fetch('http://139.196.167.75:5000/api/ManagePost/' + tiebaID, {
        method:'GET',
        headers: {
          'Accept': 'application/json',
          'Content-Type': 'application/json'
        },
      })
      .then(response => response.json())
      .then(data => this.store3(data))
      ;
      
    
    },
    


    async submitComment() {
      const data = {
        COMCONTENT: this.commentContent,
        POSTID: this.postInfo.POSTID,
        USERID: localStorage.userID
      };
    //  console.log(data);
     // console.log(JSON.stringify(data));
      console.log("userID is:"+localStorage.userID);
      if(localStorage.userID == null) {
        this.$message("请您先登录！");
      }
      else if(this.commentContent === ''){
        this.$message("请先输入评论内容");
      }
      else {
        await fetch('http://139.196.167.75:5000/api/PostTiezi', {
          method: 'POST',
          headers: {
            'Accept': 'application/json',
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(data)
        })
          .catch(error => {console.log(error)});
          this.$message("发布成功");
        location.reload();
      }
    },

    async addHistory()
    {
      const data=
      {
        USERID:localStorage.getItem("userID"),
        POSTID:this.postInfo.POSTID
      }
      await fetch('http://139.196.167.75:5000/api/History',
      {
        method: 'POST',
        headers: {
          'Accept': 'application/json',
          'Content-Type': 'application/json'
      },
      body: JSON.stringify(data)
      })
      .catch(error => {console.log(error)});

    },
    async starPost() {
      //url='http://139.196.167.75:5000/api/GetStar'
      const data = {
        POSTID: this.postInfo.POSTID,
        USERID: localStorage.userID
      };
      if(localStorage.userID == null) {
        this.$message("请先登录");
      }
      else {
        await fetch('http://139.196.167.75:5000/api/GetStar',{
          method:'POST',
          headers: {
            'Accept': 'application/json',
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(data)
        })
          .catch(error => {console.log(error)});
          this.$message("收藏成功");
          location.reload();
      }
    },


  }

}
</script>

<style>
@import url("//unpkg.com/element-ui@2.15.3/lib/theme-chalk/index.css");
  .main_body {
    margin-top: 61px;
    width: 80%;
    height: 800px;
    background-color: rgba(255, 255, 255, 0.8);
  }
  
  .userName {
    font-size:30px;
  }

  .userSig {
    display:block;
    padding:0 30px;
    font-size:15px;
  }

  .user-side {
    padding: 6% 0;
    margin-right:23px;
    border-radius:10px;
    background-color:rgba(255, 255, 255, 0.8);
    width:25% !important;
    height:100%;
    display: inline-block !important;
  }

  .content-side {
    background-color:rgba(255, 255, 255, 0.9);
    width:72% !important;
    height:100%; 
    border-radius: 10px;
    display: inline-block !important;
    font-size: 20px;
  }

  .avatar {
    height:120px;
    width:120px;
  }

  .comment {
    border-radius: 10px;
    width: 96%;
    background-color: transparent;
    height: 350px;
    margin: 0 auto;
    margin-bottom: 60px;
  }
  
  element.style {
    font-weight: bold;
  }

  .side-block {
    border-radius: 10px;
    margin-bottom: 20px;
  }

  .input{
    border:rgb(190, 185, 185) 200 solid;
    box-shadow: 0ch;
  }

  .el-row {
    margin-bottom: 0px;
    height: 60px;
  }
  .el-col {
    border-radius: 4px;
    height: 60px;
  }
  .bg-purple-dark {
    background: #99a9bf;
  }
  .bg-purple {
    background: white;
  }
  .bg-purple-light {
    background: #e5e9f2;
    height:60px;
    margin: 0;
  }
  .grid-content {
    border-radius: 4px;
    height:60px;
    line-height:60px;
    font-weight: 600;
    font-size: 15px;
    box-shadow: 0 2px 3px rgb(18 18 18 / 10%);
    margin: 2px auto;
  }
  .row-bg {
    padding: 10px 0;
    background-color: #f9fafc;
  }

  .nav {
    box-shadow: 0 3px 3px rgb(18 18 18 / 10%);
    border-bottom: #99a9bf solid 400;
    height: 60px;
  }

  * {
    margin: 0;
    padding: 0;
  }

  .item {
  margin-top: 2px;
  margin-left: 20px;
}

</style>
