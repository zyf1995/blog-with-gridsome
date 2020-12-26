<template>
  <Layout>
      <div style="min-height: 600px">
        <el-card shadow="never" style="min-height: 400px" v-if="post.id">
            <div slot="header">
                <span>{{ post.title }}</span>
            </div>
            <div style="font-size: 0.9rem;line-height: 1.5;color: #606c71;">
                发布 {{ post.created_at }}
                <br> 更新 {{ post.updated_at }}
            </div>
            <div style="font-size: 1.1rem;line-height: 1.5;color: #303133;border-bottom: 1px solid #E4E7ED;padding: 5px 0px 5px 0px">
                <pre style="font-family: '微软雅黑'">{{ post.description }}</pre>
            </div>
            <div v-html="mdToHtml(content)" class="markdown-body" style="padding-top: 20px"></div>
        </el-card>
        <el-card shadow="never" style="margin-bottom: 20px;padding: 20px 0px 20px 0px;text-align: center" v-if="!post.id">
            <font style="font-size: 30px;color:#dddddd ">
                <b>没有更新 ╮(๑•́ ₃•̀๑)╭</b>
            </font>
        </el-card>
    </div>
  </Layout>
</template>
<page-query>
query {
  posts: allStrapiPost {
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
import MarkDownIt from 'markdown-it'
const md = new MarkDownIt()
export default {
    name: "NewPage",
    metaInfo: {
        title: '最新动态'
    },
    computed: {
        post () {
            return this.$page.posts.edges[0].node
        },
        content() {
            var srcReg = /src=([\'\"]?([^\'\"]*)[\'\"]?)/ig
            return this.$page.posts.edges[0].node.content = this.$page.posts.edges[0].node.content.replace(srcReg, "src='"+this.GRIDSOME_API_URL+"$2"+"'")
        }
    },
    methods: {
        mdToHtml(mrakdown) {
            return md.render(mrakdown)
        }
    },
}
</script>

<style>

</style>