<template>
    <el-container style="min-height: 100vh;">
      <el-aside :width="sideWidth + 'px'" style="background-color: rgb(238, 241, 246); box-shadow: 2px 0 6px rgb(0 21 41 / 35%)">
        <Aside :isCollapse = "isCollapse"/>
      </el-aside>

      <el-container>
        <el-header style="border-bottom: 1px solid #ccc;" >
          <Header :collapseBtnClass="collapseBtnClass" :collapse="collapse" :user = "user"/>
        </el-header>


        <el-main>
<!--          表示当前页面的子路由会在router-view中展示-->
          <router-view @refershUser="getUser"/>
        </el-main>
      </el-container>
    </el-container>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'
import Aside from "@/components/Aside";
import Header from "@/components/Header";

export default {
  data() {
    return {
      collapseBtnClass:'el-icon-s-fold',
      isCollapse:false,
      sideWidth: 200,
      user: {}
    }
  },
  components: {
    Header,
    Aside,
  },
  created() {
    //从后台获取最新的User
    this.getUser()
  },
  methods:{
    collapse(){ //点击收缩按钮触发
      this.isCollapse = !this.isCollapse
      if(this.isCollapse){ //收缩
        this.sideWidth = 64
        this.collapseBtnClass = 'el-icon-s-unfold'
      }else{ //展开
        this.sideWidth = 200
        this.collapseBtnClass = 'el-icon-s-fold'
      }
    },
    getUser() {
      let username = localStorage.getItem("user") ? JSON.parse(localStorage.getItem("user")).username : ""
      if (username) {
        // 从后台获取User数据
        this.request.get("/user/username/" + username).then(res => {
          // 重新赋值后台的最新User数据
          this.user = res.data
        })
      }
    },
  }
}
</script>

