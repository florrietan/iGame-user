<template>
<div>
<el-table
    
    :data="FoList.slice((currentPage-1)*pagesize,currentPage*pagesize)" 
    style="width: 100%" v-loading="loading">

    <el-table-column
      prop="PASSIVEUSERID"
      label="用户ID">
    </el-table-column>
    <el-table-column
      prop="NICKNAME"
      label="用户昵称">
    </el-table-column>
    
    <el-table-column>
      <template slot-scope="scope">
              <el-button type="text" @click="lookHisInfo(scope)">查看</el-button>
            </template>
    </el-table-column>
  </el-table>

</div>
</template>


<script>
//import axios from 'axios'
  export default {
    data() {
      return {
        userID:'',
        currentPage:1,
        pagesize:10,
        FoList:[],
        loading:true,
      }
    },
  
    created(){
      this.setUserName(),
      this.getFo(this.userID),
      console.log("loaded");
      //console.log(this.userID);
    },
    methods:{
      handleSizeChange: function(val) {
            this.pagesize = val;
        },
        handleCurrentChange: function(currentPage) {
            this.currentPage = currentPage;
        },
      store:function(item){
        this.FoList=JSON.parse(item);
        

        console.log(this.FoList);
        console.log("Stored");
        },
      setUserName(){
        var storeID = localStorage.getItem("userID");
        this.userID = storeID;
      },
      async getFo(userID){     
        var url='http://139.196.167.75:5000/api/Follow'+'/'+userID;
        console.log(url);
      
          await fetch(url, {
            method: 'GET',
            headers: {
              'Accept': 'application/json',
              'Content-Type': 'application/json'
            },
          })
          .then(response => response.json())
          .then(data => this.store(data));
          this.loading=false;
          console.log("Geted");
        },
    lookHisInfo(scope)
    {
      this.$router.replace({name:'hisUserPage',query:{lookUserId:JSON.stringify(scope.row.PASSIVEUSERID)}});
      //console.log(scope);
    }
    },

    
  }
</script>