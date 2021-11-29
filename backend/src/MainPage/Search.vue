<template>
    <div class="BrowsePost" style="margin:auto;height: 800px;text-align:center; background-image: url(https://static.zhihu.com/heifetz/assets/sign_bg.db29b0fb.png); background-repeat:repeat-y;">
    <div class="back" style="height: 800px">
      <div v-if="hasInfo">
          <el-container >
                  <el-aside width="300px" style="padding:20px;"></el-aside>
                  <el-main class="AnnBack">
                      <div class="table-wrapper">
                        <template>


        <div v-if="index==0">
          <el-table
            :data="requestList.slice((currentPage-1)*pagesize,currentPage*pagesize)" 
            style="width: 100%" v-loading = "loading">
            <el-table-column
                prop="USERID"
                label="用户ID"
              >
            </el-table-column>
            <el-table-column
                prop="NICKNAME"
                label="用户昵称"
              >
              </el-table-column>

                <el-table-column >
                  <template scope="scope">
                    <el-button type="text" @click="reRes(scope,index)">查看</el-button>
                  </template>
              </el-table-column>
          </el-table>
        </div>

        <div v-else-if="index==1">
          <el-table
            :data="requestList.slice((currentPage-1)*pagesize,currentPage*pagesize)" 
            style="width: 100%" v-loading = "loading">
          <el-table-column
                prop="TIEBAID"
                label="贴吧名称"
                >
              </el-table-column>

            <el-table-column
              prop="DESCRIPTION"
              label="贴吧简介"
              >
              </el-table-column>

                <el-table-column >
                  <template scope="scope">
                    <el-button type="text" @click="reRes(scope,index)">查看</el-button>
                  </template>
              </el-table-column>
          </el-table>
        </div>

        <div v-else-if="index==2">
          <el-table
            :data="requestList.slice((currentPage-1)*pagesize,currentPage*pagesize)" 
            style="width: 100%" v-loading = "loading">

          <el-table-column
                prop="TIEBAID"
                label="贴吧名称"
              >
              </el-table-column>
        <el-table-column
                prop="POSTTITLE"
                label="帖子标题"
              >
              </el-table-column>
            

              <el-table-column >
                <template scope="scope">
                <el-button type="text" @click="reRes(scope,index)">查看</el-button>
                </template>
              </el-table-column>
          </el-table>
        </div>


          </template>
            </div>
          <el-pagination
          background
          layout="total, prev, pager, next, jumper"
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
          :current-page="currentPage"
          
          :page-size="pagesize"
          :total="requestList.length"
        >
        </el-pagination>

        </el-main>
        <el-aside width="300px" style="padding:20px;"></el-aside>
          </el-container>
      </div>
      <div v-else>
        没有要搜索的内容！
      </div>
  </div>
</div>
</template>


<script>
//import App from '../App.vue'
  export default {
    data() {
     
      return {
        name:"",
        index:'',
        requestList:[],
        IsVisible:false,
        currentPage:1,
        pagesize:10,
        loading:true,
        hasInfo:false,
      }
    },
    mounted(){
      this.getParams()
      this.getUser();
  
    },
    
    methods: {

      handleSizeChange: function(val) {
            this.pagesize = val;
        },
        handleCurrentChange: function(currentPage) {
            this.currentPage = currentPage;
        },
       getParams()
      {
        //console.log("exe");
        this.name=JSON.parse(this.$route.query.context);
        this.index=JSON.parse(this.$route.query.operation);
        //this.hasSearchContent();
        console.log("exe我们接受的参数是:"+this.name+"++"+this.index)
      },
      
     

      store:function(item){
        this.requestList=JSON.parse(item);

        if(this.requestList.length==0||this.name=='')
        {
          this.hasInfo=false;
        }
        else
        {
          this.hasInfo=true;
        }
      },
      
      reRes(scope,index)
      {
        console.log(scope);
        if(index==1)
        {
          this.$router.push({name:'BarPage',query:{barInfo:JSON.stringify(scope.row)}});
        }
        else if(index==2)
        {
          this.$router.push({name:'BrowsePost',query:{postInfo:JSON.stringify(scope.row)}})
        }
        else
        {
          this.$router.push({name:'hisUserPage',query:{lookUserId:JSON.stringify(scope.row.USERID)}})
        }
      },
      async getUser() {
        const requestData = {
          id: this.index,
          content: this.name,
        };
        console.log("Data:  "+requestData)
        await fetch('http://139.196.167.75:5000/api/Search', {
          method: 'POST',
          headers: {
            'Accept': 'application/json',
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(requestData)
        })
          .then(res => res.json())
          .then(res => this.store(res))
          .catch(error => console.log('error is', error))
          console.log("getuserGeted");
          this.loading=false;
          }
          
    },
  };
</script>

<style>

.Searchshadow{
   box-shadow: 5px 5px 5px #888888;
}
.back{
   background: rgba(255,255,255,.7);  
        border-color: rgba(255,255,255,.8);  
        border-style: solid;  
        border-width: 2px;  
}
.table{
  margin: auto;
}
.AnnBack
{
    background-color: white;
    border-radius: 20px;
     box-shadow: 10px 10px 5px #888888;
}
</style>