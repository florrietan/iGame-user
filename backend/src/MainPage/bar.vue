<template>
<div style="margin:auto;height: auto;text-align:center; background-image: url(http://pic1.win4000.com/wallpaper/2017-10-12/59df06d7c561f.jpg); background-repeat:repeat;">
    <div class="hidScroller">
  <el-container >
    
           <el-aside width="300px" style="padding:20px;height:800px" >
          <el-container class="asid-rank" style="background-color:white;height:600px">
            <el-main>

             <img src="../assets/TiebaLogo.jpg" height="120px" width="120px" style="border-radius:50%"/>
              <p><b>{{barInfo.TIEBAID}}</b></p>

              <div v-if="userIdentity==2">
              
              <el-popconfirm @confirm="dismissBar()"  title="确定要解散贴吧吗？">
                <el-button type="warning" slot="reference">解散贴吧</el-button>
              </el-popconfirm>
              
              </div>

              <div v-else-if="state==1">
              <el-button type="warning" size="small" @click="cancelFoBar()">取消关注</el-button>
              </div>

              <div v-else-if="state==0">   
              <el-button type="warning" size="small" @click="follow()">关注</el-button>
              </div>

              
              <br>

              <el-collapse >
                <el-collapse-item title="贴吧简介" name="1">
                  {{barInfo.DESCRIPTION}}
                </el-collapse-item>
                <el-collapse-item title="基本信息" name="2">
                 <p>帖子数： {{barInfo.POSTNUM}}</p>
                 <p>讨论数： {{barInfo.DISCUSSNUM}}</p>
                 <p>关注数：{{barInfo.FOLLOWNUM}}</p>
                </el-collapse-item>
                </el-collapse>


        
            </el-main>
            </el-container>

      </el-aside>

      <el-main  style="border-radius:25px;width:50%;">
        <div style="padding:20px;border-radius:25px;background-color:white"  class="back"  >
        <!--管理员-->
        <div v-if="userIdentity=='1'||userIdentity=='2'">
          <el-table
           
              :data="barData.slice((currentPage-1)*pagesize,currentPage*pagesize)" 
               v-loading="loading" @cell-dblclick="gotoPost">

              <el-table-column
                prop="POSTTIME"
                label="发帖时间">
              </el-table-column>
              <el-table-column
                prop="POSTTITLE"
                label="帖子标题">
                 
              </el-table-column>
              <el-table-column
                prop="USERID"
                label="发贴用户">
              </el-table-column>

              <el-table-column
                prop="TAGNAME"
                label="标签">
              </el-table-column>

              <el-table-column prop="POSTID">
                <template slot-scope="scope">
                  <el-popconfirm
                    title="确定删除吗？" @confirm="deletePost(scope)">
                      <el-button slot="reference" >删除</el-button>
                  </el-popconfirm>
                </template>
              </el-table-column>


            </el-table>
            <el-pagination
              background
              layout="total, prev, pager, next, jumper"
              @size-change="handleSizeChange"
              @current-change="handleCurrentChange"
              :current-page="currentPage"
              :page-size="pagesize"
              :total="barData.length">
            </el-pagination>
        </div>
        <!--非管理员-->
        <div v-else>
            <el-table
              :data="barData.slice((currentPage-1)*pagesize,currentPage*pagesize)" 
               v-loading="loading" @row-dblclick="gotoPost">

              <el-table-column
                prop="POSTTIME"
                label="发帖时间">
              </el-table-column>
              <el-table-column
                prop="POSTTITLE"
                label="帖子标题">
                
              </el-table-column>
              <el-table-column
                prop="USERID"
                label="发贴用户">
              </el-table-column>

              <el-table-column
                prop="TAGNAME"
                label="标签">
              </el-table-column>


            </el-table>
            <el-pagination
              background
              layout="total, prev, pager, next, jumper"
              @size-change="handleSizeChange"
              @current-change="handleCurrentChange"
              :current-page="currentPage"
              :page-size="pagesize"
              :total="barData.length">
            </el-pagination>
        </div>




        </div>
        <div>
        <br>
        <el-divider></el-divider>
        </div>
         <el-container  class="back" style="padding:20px;border-radius:25px;" >
           <el-main>
            
               <el-row>
            <b style="float:left">发帖</b>

            <el-select v-model="value" placeholder="请选择标签" size='medium' clearable:true>
             <el-option
                 v-for="item in tagList"
                 :key="item.TAGNAME"
                :label="item.TAGNAME"
                :value="item.TAGNAME">
             </el-option>
            </el-select>
               </el-row>
            

            
            <el-row>
                <el-input placeholder="请填写标题" v-model="postTitle" size="media" clearable> </el-input>
            </el-row>
            <br>
           
            <el-row>
                <el-input type="textarea" :rows="2" placeholder="请输入内容" v-model="postContent">  </el-input>
            </el-row>
            <el-row>
              <br> 
                <el-button type="primary" style="float:left" @click="onSubmit()">发表</el-button>
            </el-row>
           
           </el-main>
          </el-container>
      </el-main>
                <el-aside width="200px" style="padding:20px;height:800px" >
          <el-container class="asid-rank" style="background-color:white;height:700px">
            <el-main style="height:700px">

              <p><b>贴吧排行</b></p>

            
                <el-row v-for="(barItem,index) in AdRankList" :key="index" :gutter="2">
                  <span>
                    <el-col :span="4">
                      <el-button type="text" @click="toBar(barItem)">{{index+1}}</el-button>
                      </el-col>
                    <el-col :span="4">
                    <el-button type="text" @click="toBar(barItem)">{{barItem.TIEBAID}}</el-button>
                    </el-col>
                  </span>
                </el-row>
       
            </el-main>
            </el-container>

      </el-aside>
  </el-container>



    </div>
</div>

</template>

<script>
  export default {
    data() {
     
      return {
        tagList:[],
        value:'',

        IsVisible:false,
        currentPage:1,
        pagesize:8,
 
        state:0,
        userID:'',
        barID:'',
        userIdentity:0,//用户身份
        

        barData:[],
        
        barImgUrl:require('..\\assets\\1.jpg'),
        loading:true,
        postTitle:"",
        postContent:"",
        AdRankList:[],
        barInfo:{},
      }
    },
    mounted()
    {
      this.getParams();
      this.setUserID();
      this.getUserIndetity();
      //this.showFollowState();
      this.getTag();
      this.getAdRank('http://139.196.167.75:5000/api/Ranking');
      this.showFollowState();
      this.getPost();
    },
    methods:{
     


     
      handleSizeChange: function(val) {
          this.pagesize = val;
      },
      handleCurrentChange: function(currentPage) {
          this.currentPage = currentPage;
      },
      getUserIndetity()
      {
        this.userIdentity=localStorage.getItem("Identity");
      },
      setUserID(){
        var storeID = localStorage.getItem("userID");
        this.userID = storeID;
      },
       getParams()
      {
        //this.barInfo=this.$route.query.barInfo;
        this.barInfo=JSON.parse(this.$route.query.barInfo);
       // console.log(typeof this.barInfo)
       // console.log("barinfo is:"+this.barInfo)
      },
      gotoPost(row)
      {

       // console.log("gotoPost:::"+row);
        this.$router.push({name:'BrowsePost',query:{postInfo:JSON.stringify(row)}})
      },
      toBar(row)
      {
        row=JSON.stringify(row);
        //console.log(row);
        this.$router.push({name:'BarPage',query:{barInfo:row}})
        location.reload();
 
      },
       rankStore(item)
      {
        this.AdRankList=JSON.parse(item);
        // console.log(this.AdRankList);
      },


    storeState(res){
      var get=[]
      get=JSON.parse(res);
      
      this.state=get[0].NUM;
     // console.log(this.state);
     // console.log("用户状态"+this.state);
     // console.log(this.userID+this.barInfo.TIEBAID)

      },

       postStore(data)
      {
      //  console.log("storepostnow!");
        
        this.barData=JSON.parse(data);
        for(var i=0;i<this.barData.length;i++)
        {
          this.barData[i].POSTTIME= this.barData[i].POSTTIME.substring(0,10)+" " +this.barData[i].POSTTIME.substring(0,11);
        }
       // console.log("this is bardata：："+this.barData);
        
      },

      storeTag(item){
        
        this.tagList=JSON.parse(item);
       
     //   console.log(this.tagList);
    },
     
        
//接口
     async getTag()
    {
      await fetch('http://139.196.167.75:5000/api/TagA', {
            method: 'GET',
          headers: {
            'Accept': 'application/json',
            'Content-Type': 'application/json',
},
          
        })
          .then(response => response.json())
          .then(data => this.storeTag(data))
          .catch(error => {console.log(error)});
        
        },




    async onSubmit()
    {
      var user=localStorage.getItem("userID");
     if(user==null)
     {
       this.$message("请先登录")
       return;
     }
      if(this.postTitle == "")
      {
        this.$message("请输入标题！");
        return;
      }
      else if(this.postContent == ""){
        this.$message("请输入内容！");
        return;
      }
      else if(this.value==""){
        this.$message("请选择标签！");
        return;
      }
      const Data=
      {
        posttitle : this.postTitle,
        content : this.postContent,
        userid : localStorage.getItem("userID"),
        tiebaid : this.barInfo.TIEBAID,
        //TAGNAME:this.value

      }
      const data=
      {
        TAGNAME:this.value
      }
     // console.log("hello"+Data.TAGNAME);
    //  console.log(JSON.stringify(Data));
      var url='http://139.196.167.75:5000/api/ManagePost';
      var uri='http://139.196.167.75:5000/api/ManagePost/'+this.value;
      //console.log("testtttttttttt::::"+uri);
      await fetch(url, {
            method: 'POST',
            headers: {
              'Accept': 'application/json',
              'Content-Type': 'application/json',
            },
            body: JSON.stringify(Data)
          })
         .catch(error => {console.log(error)});

          await fetch(uri, {
            method: 'POST',
            headers: {
              'Accept': 'application/json',
              'Content-Type': 'application/json',
            },
            body: JSON.stringify(data)
          })
         .catch(error => {console.log(error)});
        
      
         this.postContent="";
         this.postTitle="";
         this.getPost();
         this.$message("发布成功！");
         
        //console.log("posted");
    },
      //获得所有帖子
      async getPost()
      {
        
        var url='http://139.196.167.75:5000/api/TieBaA/'+this.barInfo.TIEBAID;
        await fetch(url, {
            method: 'GET',
            headers: {
              'Accept': 'application/json',
              'Content-Type': 'application/json',
              
            },
          })
          .then(response => response.json())
          .then(data => this.postStore(data));
          this.loading=false;

      },
      //关注与否
      async showFollowState() {
      const requestData = {
          USERID: this.userID,
          TIEBAID: this.barInfo.TIEBAID,
        };
        await fetch('http://139.196.167.75:5000/api/TieBa', {
          method: 'POST',
          headers: {
            'Accept': 'application/json',
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(requestData)
        })
          .then(res => res.json())
          .then(res => this.storeState(res))
          .catch(error => console.log('error is', error));
          
          },


//关注贴吧 
      async follow()
    {
     // console.log("点击关注1");
     var user=localStorage.getItem("userID");
     if(user==null)
     {
       this.$message("请先登录")
       return;
     }
      const data={
        USERID: this.userID,
        TIEBAID: this.barInfo.TIEBAID,
      }
       await fetch('http://139.196.167.75:5000/api/FollowTieBa', {
          method: 'POST',
          headers: {
            'Accept': 'application/json',
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(data)
        })
          .catch(error => console.log('error is', error));
          
        //  console.log("点击关注");
         location.reload();

    },

//取消关注 无接口
    async cancelFoBar()
    {
      const data={
         USERID: this.userID,
        TIEBAID: this.barInfo.TIEBAID,
      }
       
       await fetch('http://139.196.167.75:5000/api/FollowTieBa', {
          method: 'DELETE',
          headers: {
            'Accept': 'application/json',
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(data)
        })
          .catch(error => console.log('error is', error));
      //    console.log("点击取消关注");
        this.followBarDecrease();
        
          location.reload();
    },
    async followBarDecrease()
    {
      await fetch('http://139.196.167.75:5000/api/FollowTieBa/' + this.barInfo.TIEBAID, {
          method: 'POST',
          headers: {
            'Accept': 'application/json',
            'Content-Type': 'application/json'
          },
        })
          .catch(error => console.log('error is', error));
      //    console.log("点击取消关注");
          location.reload();
    },
    async getBarInfoUseID()
    {
       await fetch('http://139.196.167.75:5000/api/FollowTieBa/' + this.barInfo.TIEBAID, {
          method: 'POST',
          headers: {
            'Accept': 'application/json',
            'Content-Type': 'application/json'
          },
        })
          .catch(error => console.log('error is', error));
      //    console.log("点击取消关注");
          location.reload();
    },

          //排行榜
    async getAdRank(url)
    {
      await fetch(url, {
            method: 'GET',
            headers: {
              'Accept': 'application/json',
              'Content-Type': 'application/json'
            },
          })
          .then(response => response.json())
          .then(data => this.rankStore(data));
        //  console.log("Geted");

        },
        //删除帖子
        async deletePost(scope){
            var POSTID=scope.row.POSTID;
         //   console.log("删除帖子"+POSTID);
            var url='http://139.196.167.75:5000/api/PostTiezi'+'/'+ POSTID;
          //  console.log(url);
          await fetch(url, {
            method: 'DELETE',
            headers: {
              'Accept': 'application/json',
              'Content-Type': 'application/json'
            },
          });
          location.reload();
          this.$message("删除成功！");

        },

        async dismissBar(){
            
          var url='http://139.196.167.75:5000/api/TieBaA'+'/'+ this.barInfo.TIEBAID;
            
          await fetch(url, {
            method: 'DELETE',
            headers: {
              'Accept': 'application/json',
              'Content-Type': 'application/json'
            },
          });
          
          this.$message("解散成功！");
          this.$router.push({path:'/'});

        }
    }
  };
</script>


<style>
.el-header {
    background-color: #B3C0D1;
    color: #333;
    line-height: 60px;
      border-radius: 20px;

    
  }

  .el-aside {
    color: #333;
  }
/*最外层透明*/
.el-table, .el-table__expanded-cell{
  background:  #ffffff;

}
/* 表格内背景颜色 */
.el-table th,
.el-table tr,
.el-table td {
      background: hsl(0, 0%, 100%);
}
.mainback{
     background: #40a0ff7c;
        border-color: #40a0ff7c; 
        border-style: solid;  
        border-width: 2px;  
}
.postTitle
{
    font-size:20px;
    cursor:pointer;
    font-weight:bold;
}
.postUser
{
    font-size:5px;
    cursor:pointer;
    font-weight:bold;
}
.bar-header{
   border-color: rgba(255, 255, 255, 0.829);
  background-color: rgba(255, 255, 255, 0.925);
  border-radius: 20px;
    border-style: solid;  
    border-width: 2px;  
  height: 280px;
}
.asid-rank{
  background-color:  rgba(69, 169, 250, .7);
  background: #ffffffab;
  border-radius:25px;
  height: 300px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, .12), 0 0 6px rgba(0, 0, 0, .04)

}
</style>