<template>
  <el-container class="home-container">
    <!--    头部区域-->
    <el-header>
      <div>
        <img src="../assets/logo_home.png" style="height: 40px;width: 40px">
        <span>电商后台管理系统</span>
      </div>
      <el-button type="info" @click="logout">退出</el-button>
    </el-header>
    <!--主体区域-->
    <el-container>
      <!--      测边栏-->
      <el-aside :width="isCollapse ?'64px':'200px'">
        <div class="toggle-button" @click="toggleCollapse">|||</div>
        <!--        侧边栏菜单-->
        <el-menu :collapse="isCollapse" background-color="#333744" text-color="#fff" active-text-color="#409eff"
                 :unique-opened="true" :collapse-transition="false" :router="true" :default-active="activePath">
          <!--          TODO 一級菜單-->
          <el-submenu :index="item.id" v-for="item in menuList" :key="item.id">
            <!--            一级菜单模板区-->
            <template slot="title">
              <!--              图标-->
              <i :class="iconsObj[item.id]"></i>
              <!--              文本-->
              <span>{{ item.authName }}</span>
            </template>
            <!--            TODO 二级菜单-->
            <el-menu-item :index="'/'+menuItem.path" v-for="menuItem in item.children" :key="menuItem.id"
                          @click="saveNavState('/'+menuItem.path)">
              <template slot="title">
                <!--              图标-->
                <i class="el-icon-menu"></i>
                <!--              文本-->
                <span>{{ menuItem.authName }}</span>
              </template>
            </el-menu-item>
          </el-submenu>
        </el-menu>
      </el-aside>
      <!--      右侧内容主体-->
      <el-main>
        <router-view></router-view>
      </el-main>
    </el-container>
  </el-container>

</template>

<script>
export default {
  created () {
    this.getMenuList()
    this.activePath = window.sessionStorage.getItem('activePath')
  },
  name: 'Home',
  methods: {
    logout () {
      window.sessionStorage.clear()
      this.$router.push('/login')
    },
    // 获取所有的菜单
    async getMenuList () {
      const { data: res } = await this.$http.get('menus')
      if (res.meta.status !== 200) return this.$message.error(res.meta.msg)
      res.data.map(item => {
        item.id = item.id + ''
      })
      this.menuList = res.data
      console.log(this.menuList)
    },
    toggleCollapse () {
      this.isCollapse = !this.isCollapse
    },
    // 保存理解状态
    saveNavState (activePath) {
      window.sessionStorage.setItem('activePath', activePath)
      this.activePath = activePath
    }
  },
  data () {
    return {
      // 左侧菜单数据
      menuList: [],
      iconsObj: {
        125: 'iconfont icon-user',
        103: 'iconfont icon-tijikongjian',
        101: 'iconfont icon-3702mima',
        102: 'iconfont icon-danju',
        145: 'iconfont icon-baobiao'
      },
      // 菜單是否折叠
      isCollapse: false,
      // 被激活的链接地址
      activePath: ''
    }
  }

}
</script>

<style lang="less" scoped>

.home-container {
  height: 100%;
}

.el-header {
  background-color: #373d41;
  display: flex;
  justify-content: space-between;
  padding-left: 0;
  align-items: center;
  color: #ffffff;
  font-size: 20px;

  > div {
    display: flex;
    align-items: center;
    margin-left: 15px;

    > span {
      margin-left: 15px;
    }
  }
}

.el-aside {
  background-color: #333744;

  .el-menu {
    border-right: none;
  }
}

.el-main {
  background-color: #EAEDF1;
}

.iconfont {
  margin-right: 10px;
}

.toggle-button {
  background-color: #4a5064;
  font-size: 10px;
  line-height: 24px;
  color: #ffffff;
  text-align: center;
  letter-spacing: 0.2em;
  cursor: pointer;
}
</style>
