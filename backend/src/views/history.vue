<template >
<div>
<el-table
    :data="commentList.slice((currentPage-1)*pagesize,currentPage*pagesize)" 
    border class="commit-table" v-loading = "loading" >
 
    <el-table-column
      prop="COMTIME"
      label="发布时间">
    </el-table-column>
   
    <el-table-column
      prop="COMCONTENT"
      label="评论内容">
    </el-table-column>


    <el-table-column prop="COMMENTID">
      <template slot-scope="scope">
          <el-popconfirm
              title="确定删除吗？" @confirm="deleteCom(scope)">
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
  :total="commentList.length"
>
</el-pagination>
</div>
</template>

<script>
  export default {
    data() {
      return {
        IsVisible:false,
        userID:'',
        currentPage:1,
        pagesize:5,
        commentList: [],
        loading:true,
      }
    },
    
    mounted(){
    
      this.setUserID(),this.getComment(this.userID)
    },
    methods:
    {
       handleSizeChange: function(val) {
            this.pagesize = val;
        },
        handleCurrentChange: function(currentPage) {
            this.currentPage = currentPage;
        },
       store:function(item){
        this.commentList=JSON.parse(item);
        console.log(this.commentList);
        
        },
        setUserID()
       {
         var storeID = localStorage.getItem("userID");
         this.userID = storeID;
         console.log(this.userID);
       },

      async getComment(ID){
        console.log("111"+ID);
        
        var url='http://139.196.167.75:5000/api/Comment'+'/'+ID;
        
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
      async deleteCom(scope){
            var COMMENTID=scope.row.COMMENTID;
            console.log(COMMENTID);
            var url='http://139.196.167.75:5000/api/Comment'+'/' + COMMENTID;
            console.log(url);
          await fetch(url, {
            method: 'DELETE',
            headers: {
              'Accept': 'application/json',
              'Content-Type': 'application/json'
            },
          });
          this.getComment(this.userID);
          this.$message("删除成功！");

        }

    }
  }
</script>

<style>
.commit-table{
  background-color:  rgba(69, 169, 250, .2);
  background: rgba(255,255,255,.2);  
  box-shadow: 0 4px 4px rgba(0, 0, 0, .12);

}
</style>