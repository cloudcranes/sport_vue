<template>
  <el-container class="home-container">
    <!--头部布局-->
    <el-header>
      <div>
        <img src="../assets/白兔赤乌.png" alt class="logo_img"/>
        <span>运动管理平台</span>
      </div>
      <el-button type="danger" @click="logout"><i class="el-icon-close">安全退出</i></el-button>
    </el-header>
    <el-container>
      <!--主体布局-->
      <el-main>
        <el-container>
          <!--路由占位符-->
          <el-main>
            <router-view></router-view>
          </el-main>
          <!--    底部布局-->
          <el-footer> &copy;CloudCranes</el-footer>
        </el-container>
      </el-main>

      <!--侧边布局-->
      <el-aside :width="isCollapse ?'64px':'200px'"
                style="background-image: linear-gradient(#545c64,#545c64,#0f9b0f);">
        <!--伸缩按钮-->
        <div class="toggle-button" @click="toggleCollapase"><i class="el-icon-location">导航栏</i></div>
        <!--侧边栏菜单区 unique-opened="true" 只保持一个菜单展开 router开启路由 active-text-color 颜色-->
        <el-menu


            background-color="#545c64" text-color="#fff" active-text-color="#409eff" unique-opened
            :collapse="isCollapse"
            :collapse-transition="false" :router="true" :default-active="activePath">
          <el-submenu :index="item.id+''" v-for="item in menuList" :key="item.id">
            <template slot="title">
              <i :class="iconsObject[item.id]"></i>
              <span>{{ item.title }}</span>
            </template>
            <el-menu-item :index="it.path+''" v-for="it in item.slist" :key="it.id" @click="saveNavState(it.path+'')">
              <template slot="title">
                <i :class="iconsObject[it.id]"></i>
                <span>{{ it.title }}</span>
              </template>
            </el-menu-item>
          </el-submenu>
        </el-menu>
      </el-aside>

    </el-container>

  </el-container>
</template>
<script>
export default {
  data() {
    return {
      // 右侧菜单
      menuList: [],
      iconsObject: {
        '100': 'iconfont iconguanliyuan',
        '200': 'iconfont iconsport',
        '101': 'iconfont icondenglu',
        '102': 'iconfont iconmima',
        '103': 'iconfont iconsport',
        '104': 'iconfont iconshangpin',
        '201': 'iconfont iconshu',
        '202': 'iconfont iconkaluli',
        '203': 'iconfont iconshiwu',
        '204': 'iconfont icondenglu',
      },
      isCollapse: false,
      // 被激活的连接
      activePath: '',
    }
  },
  // 类似onload
  created() {
    this.getMenuList();
    this.activePath = window.sessionStorage.getItem('activePath');// 取出session里的访问路径
  },
  methods: {
    logout() {
      window.sessionStorage.clear();
      this.$router.push("/login");
    },
    // 获取所有的导航菜单
    async getMenuList() {
      const {data: res} = await this.$http.get("menus");
      console.log(res.data);
      if (res.status != 200) return this.$message.error("操作失败！！！");
      this.menuList = res.data;
    },
    // 切换菜单折叠与展开
    toggleCollapase() {
      this.isCollapse = !this.isCollapse;
    },
    // 保存二级菜单的路径
    saveNavState(activePath) {
      window.sessionStorage.setItem('activePath', activePath);// 存放点击的二级菜单
      this.activePath = activePath;// 给点击的菜单添加高亮
    },
  }
};
</script>
<style lang="less" scoped>

.el-footer {
  overflow: hidden;
  position: fixed;
  bottom: 0;
}

.el-header {
  background-color: #373d41;
  display: flex;
  justify-content: space-between; // 左右贴边
  padding-left: 0%; // 左边界
  align-items: center; // 水平
  color: #fff;
  font-size: 20px;

  > div { //左侧div加布局
    display: flex;
    align-items: center;

    span {
      margin-left: 15px;
      background: linear-gradient(to right, #FF5F6D, #ffc371);
      -webkit-background-clip: text;
      color: transparent;
    }
  }
}

.el-aside {
  background-color: #333744;

  .el-menu {
    border-right: none; // 对其右边框
  }
}

.el-main {
  background-color: #eaedf1;
}

.home-container {
  height: 100%;
}

.logo_img {
  width: 15%;
  height: 15%;
}

.iconfont {
  margin-right: 10px;
}

.toggle-button {
  background-color: #4A5064;
  font-size: 10px;
  line-height: 24px;
  color: #fff;
  text-align: center;
  letter-spacing: 0.2em;
  cursor: pointer; // 显示鼠标指针为：小手
}
</style>
