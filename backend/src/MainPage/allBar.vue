
<template>
<div style="margin:auto;height: auto;text-align:center; background-image: url(http://pic1.win4000.com/wallpaper/2017-10-12/59df06d7c561f.jpg); background-repeat:repeat;">
    <div class="hidScroller">


  <el-container style="padding:20px">

      <el-main  style="border-radius:25px;">
        <div style="padding:20px;border-radius:25px;background-color:white"  class="back"  >
        <div>
           
            <el-table
            @row-dblclick="toBar"
              :data="AllBarData.slice((currentPage-1)*pagesize,currentPage*pagesize)" 
               v-loading="loading">
              <el-table-column
                prop="TIEBAID"
                label="贴吧名称">
              </el-table-column>
              <el-table-column
                prop="FOLLOWNUM"
                label="关注数">
              </el-table-column>
              <el-table-column
                prop="POSTNUM"
                label="发帖数">
              </el-table-column>
            </el-table>
             <el-pagination
               background
               layout="total, prev, pager, next, jumper"
               @size-change="handleSizeChange"
               @current-change="handleCurrentChange"
               :current-page="currentPage"
  
               :page-size="pagesize"
               :total="AllBarData.length"
>
</el-pagination>
            
           
        </div>
        </div>
      </el-main>
      <el-aside>
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
        <div>
        <br>
        </div>
        </el-container>
    </div>
</div>
        
            
  
</template>

<script>
  export default {
    data() {
     
      return {
        IsVisible:false,
        currentPage:1,
        pagesize:9,

        AllBarData:[],
        barTitle:"1",
        
        loading:true,
        AdRankList:[],
      }
    },
    mounted()
    {
      //this.getParams();
      this.getAllBarData();
      this.getAdRank('http://139.196.167.75:5000/api/Ranking');
    },
    methods:{
      handleSizeChange: function(val) {
            this.pagesize = val;
        },
        handleCurrentChange: function(currentPage) {
            this.currentPage = currentPage;
        },
       rankStore(item)
    {
      this.AdRankList=JSON.parse(item);
      //console.log(this.AdRankList);
    },
    toBar(row)
    {
        
        var info=JSON.stringify(row);
        //console.log(info);
        //console.log("gotAllBar");
        this.$router.push({name:'BarPage',query:{barInfo:info}})
      
    },
    storeAllBarData(data)
    {
        this.AllBarData=JSON.parse(data);
        //console.log(this.AllBarData);
    },
      async getAllBarData()
      {
        var url='http://139.196.167.75:5000/api/TieBa';
       // console.log(url);
        await fetch(url, {
            method: 'GET',
            headers: {
              'Accept': 'application/json',
              'Content-Type': 'application/json',
            },
          })
          .then(response => response.json())
          .then(data => this.storeAllBarData(data));
          this.loading=false;
        //  console.log("Geted");

      },
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
         // console.log("Geted");

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
