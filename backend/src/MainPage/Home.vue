<template>
    <div style="margin:auto;height: auto;text-align:center; background-image: url(https://static.zhihu.com/heifetz/assets/sign_bg.db29b0fb.png); background-repeat:repeat;">
    <div class="Home-back" style="padding:20px;padding-top:0;">
      <br>
     
     <el-container class="Home-header" style="margin-bottom:50px;">
       <el-main class="Home-header" style="overflow-x:hidden;">
         <el-row :gutter= '2'>
           <el-col :span= "15">
        <el-carousel height="200px" type="card" >
          <el-carousel-item v-for="(item,index) in hotbar" :key="index" class="Home-shadow">

          <img :src = "item.viewRouter" height="100%"/>
          </el-carousel-item>
        </el-carousel>
        </el-col>
        <el-col :span="8" style="float:right;" height="300px">   
          <b>公告栏</b>
          <br>
      
          <div v-for="(AdItem,index) in AdList" :key="index" >
           <div v-if= isShow(index)>
              <el-row class="Home-shadow" style="height:40px">
                    <el-button type="text" @click= "dialogClick(AdList[index])" id="index"> {{AdItem.ANNOUNCEMENTTITLE}} </el-button>           
                    <el-dialog :title="Title" :visible.sync = "dialogVisible">
                        <p>{{Content}}</p>
                    </el-dialog>
              </el-row>
             </div>
          </div>
            <el-button type="text" style="float:right" @click="toMoreAnn()"> 更多 </el-button>        
        </el-col>
         </el-row>
        </el-main>
     </el-container>
     
     <div class="Home-el-scrollbar__wrap">
       <br>
        <el-container>

      
      
      <div class="Home-el-scrollbar__wrap">
        <el-aside width="270px" class="Home-left" style="float:left;height:auto;border-radius:15px;">
            <!--此处修改，可能有跳转bug-->
            <div style="color:#2581d6;font-size:25px;height:50px;font-weight:900;margin-top:20px;padding:10px;">
              贴吧热榜
            </div>
            <el-table :data="AdRankList" @row-click="toBar" stripe height="550px" style="width: 100%;">
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

        </el-aside>
      </div>

      <div class="Home-flex-content" style="margin-left:100px;">
        <el-main style="width:auto;height:1000px;overflow: hidden">
          <div >
                <el-row>
                      <el-col :span="5">
                        <b>推荐贴吧</b>
                      </el-col>
                      <el-col :span="5">
                          <div style="font-size:2px" class="Home-barTitle" >
                            <b @click="toAllBar()">更多</b>
                          </div>
                      </el-col>
                </el-row> 
            <br>
            <el-row style="height:250px;width:1000px">
                <el-col :span="10" v-for="(item, index) in AdRankList" :key="index" :offset="index % 2>0?2:0" style="height:200px">
                  <div v-if="index<4">
                  <el-card :body-style="{ padding: '0px' }" style="height:190px">
                    
                    <div style="padding: 14px;">
                      <span></span>
                      <h4>{{item.TIEBAID}}</h4>
                      <br />
                      <img src="../assets/TiebaLogo.jpg" style="width:100px;height:100px"/>
                      <div>
                        <el-row>
                              <el-button type="text" class="button" @click="toBar(item)">打开贴吧</el-button>

                        </el-row>
                      </div>
                    </div>
                  </el-card>
                  </div>
                </el-col>
              </el-row>
          </div>
          <br>
          
        </el-main>
      </div>
     </el-container>
    
     </div>
    </div>
  </div>

  
</template>

<script>
export default {
  data() {
      return {
        dialogVisible : false,
        Title : " ",
        Content : " ",
        IsLogin: true,
        
        circleUrl:"https://cube.elemecdn.com/3/7c/3ea6beec64369c2642b92c6726f1epng.png",
        myBarList:[],
        barList:[],
        hotbar:[{
          viewRouter: require('../assets/4.jpg'),
        },
        {
          viewRouter : require('../assets/2.jpg'),
        },
        {
          viewRouter : require('../assets/3.webp'),
        }],
        bar: [],
        AdList:[{},],
        AdRankList:[],
      }
  },
  mounted(){
    this.setUserID();
 
    this.getAd('http://139.196.167.75:5000/api/Announcement');
    this.getAdRank('http://139.196.167.75:5000/api/Ranking');
       this.getMyBar(this.UserID);
  },
  methods:{
    setUserID()
    {
      var storeID = localStorage.getItem("userID");
      this.UserID = storeID;
    },
    storeMyBar(respons)
    {
      this.barList = JSON.parse(respons);
      this.myBarList[0]=this.barList[0];
      this.myBarList[1]=this.barList[1];
   //   console.log(this.myBarList[0]);
      
      //console.log(this.myBarList);
    },
   
    toBar(row)
    {
        row=JSON.stringify(row);
        console.log(row);
        this.$router.push({name:'BarPage',query:{barInfo:row}})
    },
    toAllBar()
    {
      this.$router.replace({path:'/allBar'});
    },
    toMoreAnn()
    {
      this.$router.replace({path:'/Ann'});
    },
    toMyBar(){
      this.$router.replace({path:'/followBar'});

    },


    
    dialogClick:function(AdItem)
    {
      this.dialogVisible=true;
      this.Title=AdItem.ANNOUNCEMENTTITLE;
      this.Content=AdItem.ANNOUNCEMENTCONTENT;
   //   console.log(this.dialogVisible);
    },
    isShow:function(index){
      return index > 3 ? false: true;
      },
    checkLogin:function()
    {
      return localStorage.getItem("state");
    },
    adStore:function(item)
    {
        this.AdList=JSON.parse(item);
      //  console.log(this.AdList);
    },
    rankStore(item)
    {
      this.AdRankList=JSON.parse(item);
    //  console.log(this.AdRankList);
    },

    //接口
    //获得关注
    async getMyBar(params)
    {
      var url='http://139.196.167.75:5000/api/FollowTieBa'+'/'+params;
      await fetch(url, {
          method: 'GET',
          headers: {
            'Accept': 'application/json',
            'Content-Type': 'application/json'
          },
         
        })
        .then(respons => respons.json())
        .then(respons => this.storeMyBar(respons));
      
        console.log("获得关注")
    },
    async getAd(url)
    {
      await fetch(url, {
            method: 'GET',
            headers: {
              'Accept': 'application/json',
              'Content-Type': 'application/json'
            },
          })
          .then(response => response.json())
          .then(data => this.adStore(data));
          console.log("Geted");
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
          console.log("Geted");

        },

        
    }
}
    

</script>

<style>
.Home-back{
   background: rgba(255,255,255,.7);  
        border-color: rgba(255,255,255,.8);  
        border-style: solid;  
        border-width: 2px;  
}
.Home-header{
   border-color: rgba(255, 255, 255, 0.288);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 20px;
    border-style: solid;  
    border-width: 2px;  
  height: 280px;
}
.Home-smallHeader{
  background-color: #2c84db80;
  
}
.Home-asid-rank{
  background-color:  rgba(69, 169, 250, .7);
  background: #ffffffab;
  border-radius:25px;
  height: 300px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, .12), 0 0 6px rgba(0, 0, 0, .04)

}
.Home-shadow{
   box-shadow: 0 2px 4px rgba(0, 0, 0, .12), 0 0 6px rgba(0, 0, 0, .04)
}
.Home-left{
    background-color:  rgba(255, 255, 255, 0.6);
}
.Home-inline-input{
  width: 800px;
  
}
.Home-block
{
  height: 1000px;
  border-radius: 10px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, .12), 0 0 6px rgba(0, 0, 0, .04);
   background-color:  rgba(255, 255, 255, 0.4);

}
.Home-barTitle
{
    font-size:40px;
    cursor:pointer;
    font-weight:bold;
}
.Home-barTitle:hover
{
  color:rgba(4, 121, 255, 0.568);
}
.Home-el-scrollbar__wrap {
    overflow-x: hidden;
    overflow-y: hidden;
    
}
</style>