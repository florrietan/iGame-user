<template>
<div style="padding:20px;border-radius:25px;background-color:white"  class="back"  >
                  <div>
                    
                      <el-table
                        :data="AllMessage"  v-loading="loading"
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
</template>
<script>
  export default {
    data() {
     
      return {
        loading:true,
        
        AllMessage:[],
      }
    },
    mounted()
    {
      //this.getParams();
      
      //this.UserInfo=JSON.parse(this.$route.UserInfo);
      this.getAllMessage(localStorage.getItem("userID"));

    },
    methods:{
    storeAllMessage(data)
    {
        this.AllMessage=JSON.parse(data);
        console.log(this.AllMessage);
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
          this.loading=false;
      },



      
    }
  };
</script>