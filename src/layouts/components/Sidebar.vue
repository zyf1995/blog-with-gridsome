<template>
  <div>
    <el-card shadow="never">
      <el-menu :default-active="active" @select="onSelect">
        <el-menu-item
          v-for="item in constantRouterMap"
          :key="item.path"
          :index="item.path"
        >
          <i :class="item.meta.icon"></i>
          <span slot="title">{{ item.meta.title }}</span>
        </el-menu-item>
      </el-menu>
    </el-card>

    <el-card shadow="never" style="margin-top: 20px; text-align: center">
      <div
        v-if="!token"
        style="font-size: 0.9rem; line-height: 1.5; color: #606c71"
      >
        <el-tag type="danger" size="small">&nbsp;</el-tag>&nbsp;&nbsp;
        Token未绑定&nbsp;&nbsp;
        <el-button type="text">绑定</el-button>
      </div>
      <div
        v-if="token"
        style="font-size: 0.9rem; line-height: 1.5; color: #303133"
      >
        <el-tag type="success" size="small">&nbsp;</el-tag>&nbsp;&nbsp;
        Token已绑定&nbsp;&nbsp;
        <el-button type="text">注销</el-button>
      </div>
      <div style="margin-top: 10px; text-align: left">
        <el-alert
          title="Token获取"
          type="info"
          description="在 github-> settings-> developerSettings-> personalAccessTokens 勾选gist权限,获取Token. 详情参考README.md"
          :closable="false"
        >
        </el-alert>
      </div>
    </el-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      constantRouterMap: [
        {
          path: "/new",
          meta: {
            icon: "el-icon-star-off",
            title: "最新动态",
          }
        },
        {
          path: "/social",
          meta: {
            icon: "el-icon-mobile-phone",
            title: "社交圈",
          }
        },
        {
          path: "/blog",
          meta: {
            icon: "el-icon-edit-outline",
            title: "博客列表",
          }
        },
        {
          path: "/project",
          meta: {
            icon: "el-icon-service",
            title: "开源项目",
          }
        },
      ],
      active: "",
      token: false
    }
  },
  mounted () {
    let arr = this.$route.path.split("/")
    this.active = "/" + arr[1]
  },
  methods: {
      onSelect(index) {
        if(this.active == index) {
          return
        }
        this.$router.push(index)
      }
  }
};
</script>