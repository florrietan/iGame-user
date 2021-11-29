
<template>
<div >
    <div class="hidScroller">
      <div>
   
          <el-container  style="height: 500px; border: 1px solid #eee">
              <el-aside width="300px" class="asid">
                <br>
                  <img src="..\assets\1.jpg" height="180px" width="180px" style="border-radius:50%"/>
                  <p><b> {{UserInfo.NICKNAME}} </b></p>
                  <p>个人简介： {{UserInfo.RELATIONSHIPTYPE}} </p>
                    <el-menu   :default-active="$path='/UserPage'" :collapse-transition="true">
                    <el-menu-item >他的留言板</el-menu-item>
                    <el-menu-item @click="gotoInfo()">他的个人资料</el-menu-item>
                    </el-menu>
                    <div v-if = "isLogin">
                          <div v-if = "isFollow">
                              <el-popconfirm @confirm="disFollowUser()"  title="确定要取消关注吗？">
                              <el-button  slot="reference">取消关注</el-button>
                            </el-popconfirm>
                          </div>
                          <div v-else>
                            <el-button @click="followUser()">关注</el-button>
                          </div>
                          <br />
                        <el-button @click="dialogFormVisible = true">添加留言</el-button>

                            <el-dialog title="添加留言" :visible.sync="dialogFormVisible">
                                    <el-input v-model="mesContent" type="textarea" :rows="3"></el-input>
                              <div slot="footer" class="dialog-footer">
                                <el-button @click="dialogFormVisible = false">取 消</el-button>
                                <el-button type="primary" @click="leaveMess()">确 定</el-button>
                              </div>
                            </el-dialog>
                    </div>
              </el-aside>
          <el-main >
            <el-container style="padding:20px">

                <el-main  style="border-radius:25px;">
                  <div style="padding:20px;border-radius:25px;background-color:white"  class="back"  >
                  <div>
                    
                      <el-table
                        :data="AllMessage" 
                        >
                        <el-table-column
                          prop="USERID"
                          label="留言者">
                        </el-table-column>
                        <el-table-column
                          prop="CONTENT"
                          label="留言">
                        </el-table-column>

                      </el-table>
                    
                  </div>
                  </div>
                </el-main>
                
                  <div>
                  <br>
                  </div>
                  </el-container>
          </el-main>
          </el-container>
      </div>
    </div>
</div>
        
            
  
</template>

<script>
  export default {
    data() {
     
      return {
        AllBarData:[],
        barTitle:"1",
        currentPage:1,
        pagesize:5,
        loading:true,
        AdRankList:[],
        AllMessage:[],
        UserInfo:[],
        isFollow:false,  
        dialogFormVisible:false,
        mesContent:"",
        isLogin:false
      }
    },
    mounted()
    {
      this.getParams();
      this.isLogin=JSON.parse(this.$route.query.isLogin)
      //this.UserInfo=JSON.parse(this.$route.UserInfo);
      this.getAllMessage(this.UserInfo.USERID);

    },
    methods:{
    storeAllMessage(data)
    {
        this.AllMessage=JSON.parse(data);
      //  console.log(this.AllMessage);
    },
    async leaveMess()
    {

      this.dialogFormVisible = false;
      const data={
        CONTENT:this.mesContent,
        USERID:localStorage.getItem("userID"),
        ACUSERID:this.UserInfo.USERID,
      }
      //console.log(data);
      var url='http://139.196.167.75:5000/api/Message'
      await fetch(url, {
          method: 'POST',
          headers: {
            'Accept': 'application/json',
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(data)
        })
        this.getAllMessage(this.UserInfo.USERID);
    },
    getParams()
    {
      //var data=this.$route.query;
      //console.log(data);
      this.UserInfo=JSON.parse(this.$route.query.UserInfo);
      this.isFollow=JSON.parse(this.$route.query.isFollow)
     // console.log(this.UserInfo)
     // console.log(this.isFollow)
    },
      async getAllMessage(params)
      {
        var url='http://139.196.167.75:5000/api/Message'+'/'+params;
        await fetch(url, {
            method: 'GET',
            headers: {
              'Accept': 'application/json',
              'Content-Type': 'application/json'
            },
          })
          .then(respons => respons.json())
          .then(data => this.storeAllMessage(data));
      },
      async followUser()
    {
      const data={
        PID:this.lookUserId,
        AID:this.UserID
      }
       var url='http://139.196.167.75:5000/api/Follow';
       await fetch(url, {
          method: 'POST',
          headers: {
            'Accept': 'application/json',
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(data)
        })
        const query=JSON.parse(JSON.stringify(this.$route.query))
        query.isFollow=true;//state 修改的参数
        this.$router.push({ path: this.$route.path, query })
        location.reload();

    },
    async disFollowUser()
    {
      const data={
        PID:this.lookUserId,
        AID:this.UserID
      }
      var url='http://139.196.167.75:5000/api/Follow';
       await fetch(url, {
          method: 'DELETE',
          headers: {
            'Accept': 'application/json',
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(data)
        })
        location.reload();
        const query=JSON.parse(JSON.stringify(this.$route.query))
        query.isFollow=false;//state 修改的参数
        this.$router.push({ path: this.$route.path, query })
        location.reload();
    },
    gotoInfo()
    {
      this.$router.replace({name:'hisUserPage',query:{lookUserId:JSON.stringify(this.UserInfo.USERID)}});
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
</style>
