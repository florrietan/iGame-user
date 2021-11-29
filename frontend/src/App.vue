<template>

  <div id="app">
    <el-row v-if="!$route.meta.ifShow">
  <div>
    <el-row >
      <el-col class="Header">
        <el-col :span="5">
          <div class="grid-content bg-purple" style="height:60px;margin:0;">
            <a style="" href="/#/">
              <img class="logo" height="58px" width="170px" src="https://wx1.sinaimg.cn/orj360/4f5151f6gy1gsdcsyhuj6j21ez0ron0r.jpg">
            </a>
          </div>        
        </el-col>
        <el-col :span="4" style="padding:15px">
                <el-radio-group v-model="radio" size="mini">
                    <el-radio-button label="用户"></el-radio-button>
                    <el-radio-button label="贴吧"></el-radio-button>
                    <el-radio-button label="帖子"></el-radio-button>
                  </el-radio-group>
                      </el-col>
        
        <el-col :span="6" style="padding:10px">
          
          <div>
        <el-input  placeholder="请输入内容" style="width:60%;margin-right:10px;"></el-input>
        <el-button type="primary" icon="el-icon-search" @click="search()" style="height:80%;border-radius:5px;font-size:15px;">搜索</el-button>
          </div>
        </el-col>
    <el-menu mode="horizontal" :router = "true" background-color="#2e78c2" text-color="white" active-text-color="yellow" style="float:right">
      <el-menu-item index="/">首页</el-menu-item>
        <el-submenu index="4" v-if = "UserState">
                <template slot="title">贴吧</template>
                <el-menu-item index="/barPage">查看贴吧</el-menu-item>
                <el-menu-item index="/creatBar">创建贴吧</el-menu-item>
        </el-submenu>
        <el-menu-item  v-else index="/barPage">查看贴吧</el-menu-item>

        <el-submenu index="1" v-if = "userIdentity == 2">
         <template #title>公告</template>

            <el-menu-item index="/Ann">查看公告</el-menu-item>
            <el-menu-item index="/AddAn">增加公告</el-menu-item>
    
        </el-submenu>
        <el-menu-item index="/Ann" v-if = "userIdentity != 2">查看公告</el-menu-item>

        <el-menu-item v-if = "!UserState"  index="/Login">登录/注册</el-menu-item>

       <el-submenu index="3" v-if = "UserState">
         <template #title>个人中心</template>

            <el-menu-item index="/activity">个人主页</el-menu-item>
            <el-menu-item index="Logout" @click="Logout()">注销</el-menu-item>
    
        </el-submenu>

    </el-menu>
    </el-col>
  </el-row>
  </div>
    </el-row>
      <router-view />
  </div>
  
</template>
<script>
export default {
  mounted()
  {
    this.setUserState();
    this.getUserIndetity();
  },
  methods:{
     search:function() {
        this.$router.replace({path: '/SearchPage'});
      },
    setUserState()
    {
      var state = localStorage.getItem("state");
      this.UserState = state;
    },
    getUserIndetity()
    {
      this.userIdentity=localStorage.getItem("Identity");
    },
    
    Logout()
    {
      localStorage.clear();
      this.$router.replace({path: '/redirct'});
      location.reload();
    }
  },
  data(){
    return{
      UserState:false,
      userIdentity:0,
      radio:"用户"
    }
  }
}
</script>
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
.Header{
  background-color: #2e78c2;

}
.el-dropdown-link
{
  color:white;
  cursor: pointer;
  float: right;
}

</style>
