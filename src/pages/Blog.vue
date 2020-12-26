<template>
  <Layout>
      <div style="min-height: 600px">
        <el-card shadow="never" style="margin-bottom: 20px">
            <el-input placeholder="请输入关键字" v-model="searchKey" clearable style="width: 300px"></el-input>
            <el-button @click="search" icon="el-icon-search" style="margin-left: 10px" circle plain></el-button>
            <el-button @click="$share()" style="margin-left: 10px" icon="el-icon-share" type="warning" plain circle></el-button>
            <el-button type="primary" icon="el-icon-edit" round plain style="float: right;" @click="goAdd">写博文</el-button>
        </el-card>

        <div v-if="$page.posts.edges && $page.posts.edges.length>0">
            <el-card shadow="hover" v-for="(item,index) in $page.posts.edges" :key="'p'+index" style="margin-bottom: 20px" v-if="!item.node.hide">
                <div slot="header">
                    <el-row>
                        <el-col :span="16">
                            <span>
                                <g-link 
                                    style="text-decoration:none;cursor:pointer" 
                                    :to="'blog/detail/' + item.node.id"
                                    >
                                    <i class="el-icon-edit-outline"></i>&nbsp;&nbsp; {{item.node.title}}
                                </g-link>
                            </span>
                        </el-col>
                        <el-col :span="8">
                            <div style="text-align: right;">
                                <el-button @click="$share('/user/blog/details/'+item.node.id)" style="padding: 3px 0" type="text" icon="el-icon-share"></el-button>
                                <el-button @click="editBlog(index)" style="padding: 3px 0" type="text" icon="el-icon-edit" v-if="token"></el-button>
                                <el-button @click="deleteBlog(index)" style="padding: 3px 0" type="text" icon="el-icon-delete" v-if="token"></el-button>
                            </div>
                        </el-col>
                    </el-row>
                </div>
                <div style="font-size: 0.9rem;line-height: 1.5;color: #606c71;">
                    最近更新 {{item.node.updated_at}}
                </div>
                <div style="font-size: 1.1rem;line-height: 1.5;color: #303133;padding: 10px 0px 0px 0px">
                    {{item.node.description}}
                </div>
            </el-card>
            <div style="text-align: center">
                <!-- <el-pagination @current-change="list" background layout="prev, pager, next" :current-page.sync="$page.posts.pageInfo" :page-size="query.pageSize"
                    :total="query.pageNumber*query.pageSize">
                </el-pagination> -->
                <Pager :info="$page.posts.pageInfo"/>
            </div>

        </div>

        <el-card shadow="never" style="margin-bottom: 20px;padding: 20px 0px 20px 0px;text-align: center" v-if="!$page.posts.edges || $page.posts.edges.length==0">
            <font style="font-size: 30px;color:#dddddd ">
                <b>还没有博客 (╯°Д°)╯︵ ┻━┻</b>
            </font>
        </el-card>
    </div>
  </Layout>
</template>
<page-query>
query ($page: Int) {
  posts: allStrapiPost (perPage: 2, page: $page) @paginate {
    pageInfo {
      totalPages
      currentPage
    }
    edges {
      node {
        id
        title
        description
        content
        created_at
        updated_at
      }
    }
  }
}
</page-query> 
<script>
import { Pager } from 'gridsome'
export default {
    name: "BlogPage",
    metaInfo: {
      title: '博客列表'
    },
    components: {
        Pager
    },
    data() {
        return {
            token: false,
            searchKey: ""
        }
    },
    methods: {
      list() {
        console.log('获取数据')
      },
      search() {
          for (let i = 0; i < this.blogs.length; i++) {
              this.blogs[i].hide = this.blogs[i].title.indexOf(this.searchKey) < 0
          }
      },
      editBlog(index) {
          if (!this.token) {
              this.$message({
                  message: '请绑定有效的Token',
                  type: 'warning'
              })
              return
          }
          this.$router.push('/user/blog/edit/' + this.blogs[index].id)
      },
      deleteBlog(index) {
          this.$confirm('是否永久删除该博客?', '提示', {
              confirmButtonText: '确定',
              cancelButtonText: '取消',
              type: 'warning'
          }).then(() => {
              let blog = this.blogs[index]
              GistApi.delete(blog.id).then((result) => {
                  this.$message({
                      message: '删除成功',
                      type: 'success'
                  })
                  this.blogs.splice(index, 1)
              })
          })
      },
      goAdd() {
          if (!this.token) {
              this.$message({
                  message: '请绑定有效的Token',
                  type: 'warning'
              })
              return
          }
          this.$router.push('/user/blog/add')
      }
    }
}
</script>

<style>

</style>