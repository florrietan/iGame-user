<template>
    <div style="margin:auto;height:800px;text-align:center; background-image:url(https://static.zhihu.com/heifetz/assets/sign_bg.db29b0fb.png); background-repeat:repeat;">
        <div class="back" style="height:800px">
        <el-container style="height:500px;padding:30px">
          
            <el-aside> </el-aside>
            <el-main  style="padding:20px">

              <div v-if="hasTag">
             <el-row>
              <el-col :span="4" v-for="(item, index) in tagList" :key="index" :offset="index % 4 > 0 ? 2 : 0" style="height:100px">
                <el-card style="height:80px">
      
                 
                  <span></span>
                   <h4>{{item.TAGNAME}}</h4>                
                   <div class="bottom clearfix">
                      <el-popconfirm @confirm="deleteTag(item.TAGNAME)"  title="确定删除吗？">
                     <el-button type="text" slot="reference" >删除标签</el-button>
                    </el-popconfirm>
                   </div>
           
               </el-card>
              </el-col>
             </el-row>
              </div>

              <div v-else>
                <h4>没有标签</h4>
                  </div>


           </el-main>
            <el-aside> </el-aside>
        </el-container>

        <el-container style="padding:30px">
            <el-aside> </el-aside>
            <el-main class="AddBack Adshadow" style="padding:15px">
            <b style>创建标签</b>
            <el-row>
                <br>
                <el-input type="text" placeholder="请输入标签名称" v-model= "Content"></el-input>
            </el-row>
            <br>
            <el-row>
              <br>
                <el-button type="primary" class="shadow" style="float:left;background:white;color:#318de2" @click = "onSubmit()">发布标签</el-button>
            </el-row>
           </el-main>
            <el-aside> </el-aside>
        </el-container>
        </div>
  </div>

  
</template>

<script>

export default {
  data() {
      return {
        tagList:[],
        TagPost:[],
        Content:'',
        hasTag:true,
      }
  },

  mounted()
    {
      
      this.getTag();
     
    },

  methods:{
     storeTag(item){
        
        this.tagList=JSON.parse(item);
    },
    storeTagPost(item){
        
        this.TagPost=JSON.parse(item);
    },
     
        

    async getTag()
    {
      await fetch('http://139.196.167.75:5000/api/TagA', {
            method: 'GET',
          headers: {
            'Accept': 'application/json',
            'Content-Type': 'application/json'
          },
          
        })
          .then(response => response.json())
          .then(data => this.storeTag(data))
          .catch(error => {console.log(error)});
         if(this.tagList.length==0){
        this.hasTag=false;
       }
        else{
        this.hasTag=true;
       }
        },

        async getTagPost(tagNeme)
    {
      await fetch('http://139.196.167.75:5000/api/TagA/'+tagNeme, {
            method: 'GET',
          headers: {
            'Accept': 'application/json',
            'Content-Type': 'application/json'
          },
          
        })
          .then(response => response.json())
          .then(data => this.storeTagPost(data))
          .catch(error => {console.log(error)});
        
        },

    async onSubmit()
    {

      if(this.Content=="")
      {
        this.$message("请输入标签名");
          return;
      }
      for(var i=0;i<this.tagList.length;i++){
        if(this.tagList[i].TAGNAME==this.Content){
          this.$message("标签已存在");
          return;
          }
      }

        const data=
        {
           
          TAGNAME:this.Content,
        };
        console.log(data);
        console.log(JSON.stringify(data));
      await fetch('http://139.196.167.75:5000/api/TagA', {
            method: 'POST',
          headers: {
            'Accept': 'application/json',
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(data)
        })
          .catch(error => {console.log(error)});
          //this.$router.replace({path:'/Ann'});
          this.Content="";
        
          this.getTag();
          this.$message("发布成功");

        },

       
    async deleteTag(tagNeme){
      this.getTagPost(tagNeme);
      console.log(this.TagPost)
      if(this.TagPost.length!=0)
      {
        this.$message("该标签下有帖子，无法删除！")

      }
      else{
       const data={
         
        TAGNAMME: tagNeme,
      }
          await fetch('http://139.196.167.75:5000/api/TagA/'+tagNeme, {
            method: 'DELETE',
            headers: {
              'Accept': 'application/json',
              'Content-Type': 'application/json'
            },
            body: JSON.stringify(data)
          });
         
          this.getTag();

        }
        },
    }

  }

</script>

<style>
.AddBack{
   background: rgba(87, 167, 199, 0.363);
    border-radius: 20px;
    border-width: 2px; 
}
.Adshadow{
   box-shadow: 10px 10px 5px #888888;
}
.back{
   background: rgba(255,255,255,.7);  
    border-style: solid;  
    border-width: 2px;  
}
</style>