<template>
  <Layout>
      <el-card shadow="never" style="min-height: 400px;margin-bottom: 20px;padding: 0px 0px 20px 0px">
          <el-tabs v-model="activeTab" type="card" @tab-click="onSelect">
              <el-tab-pane :label="'粉丝 '+$page.fans.totalCount" name="followers" style="padding: 5px">
                  <div>
                      <div v-if="$page.fans.edges.length">
                          <el-row style="min-height: 200px; ">
                              <el-col :span="8" v-for="(item,index) in $page.fans.edges" :key="'followers'+index" style="padding: 10px">
                                  <el-card shadow="hover" style="font-size: 13px;color: #606266;line-height: 20px">
                                      <i class="el-icon-star-off"></i>&emsp;
                                      <a style=" text-decoration:none;cursor:pointer">{{item.node.name}}</a>
                                      <br>
                                      <i class="el-icon-message"></i>&emsp;
                                      <a :href="item.node.htmlUrl" target="_blank" style=" text-decoration:none;cursor:pointer">TA的主页</a>
                                      <br>
                                      <img :src="GRIDSOME_API_URL + item.node.avatarUrl.url" style="width: 100%;border-radius:5px;margin-top: 5px">
                                  </el-card>
                              </el-col>
                          </el-row>
                          <div style="text-align: center;margin-top: 10px">
                              <el-pagination @current-change="onSelect" background layout="prev, pager, next" :current-page.sync="followers.query.page"
                                  :page-size="followers.query.pageSize" :total="followers.query.pageNumber*followers.query.pageSize">
                              </el-pagination>
                          </div>
                      </div>
                      <div style="min-height: 300px;margin-bottom: 20px;padding: 20px 0px 20px 0px;text-align: center" v-else>
                          <font style="font-size: 30px;color:#dddddd ">
                              <b>(￢_￢) 没有一个粉丝</b>
                          </font>
                      </div>
                  </div>
              </el-tab-pane>
              <el-tab-pane :label="'关注 '+$page.attentions.totalCount" name="following" style="padding: 5px">
                  <div v-loading="following.loading">
                      <div v-if="following.list.length">
                          <el-row style="min-height: 200px; ">
                              <el-col :span="8" v-for="(item,index) in $page.attentions.edges" :key="'following'+index" style="padding: 10px">
                                  <el-card shadow="hover" style="font-size: 13px;color: #606266;line-height: 20px">
                                      <i class="el-icon-star-off"></i>&emsp;
                                      <a style=" text-decoration:none;cursor:pointer">{{item.node.name}}</a>
                                      <br>
                                      <i class="el-icon-message"></i>&emsp;
                                      <a :href="item.node.htmlUrl" target="_blank" style=" text-decoration:none;cursor:pointer">TA的主页</a>
                                      <br>
                                      <img :src="GRIDSOME_API_URL + item.node.avatarUrl.url" style="width: 100%;border-radius:5px;margin-top: 5px">
                                  </el-card>
                              </el-col>
                          </el-row>
                          <div style="text-align: center;margin-top: 10px">
                              <el-pagination @current-change="onSelect" background layout="prev, pager, next" :current-page.sync="following.query.page"
                                  :page-size="following.query.pageSize" :total="following.query.pageNumber*following.query.pageSize">
                              </el-pagination>
                          </div>
                      </div>
                      <div style="min-height: 300px;margin-bottom: 20px;padding: 20px 0px 20px 0px;text-align: center" v-else>
                          <font style="font-size: 30px;color:#dddddd ">
                              <b>(￢_￢) 还没有关注一个人</b>
                          </font>
                      </div>
                  </div>
              </el-tab-pane>
          </el-tabs>
      </el-card>
  </Layout>
</template>
<page-query>
query ($page: Int) {
  fans: allStrapiFan (perPage: 9, page: $page) @paginate {
    pageInfo {
      totalPages
      currentPage
    }
    totalCount
    edges {
      node {
        id
      	name
        htmlUrl
        avatarUrl {
            url
        }
      }
    }
  }
  attentions: allStrapiAttention (perPage: 9, page: $page) @paginate {
    pageInfo {
      totalPages
      currentPage
    }
    totalCount
    edges {
      node {
        id
      	name
        htmlUrl
        avatarUrl{
            url
        }
      }
    }
  }
}
</page-query> 
<script>
export default {
    name: "SocialPage",
    metaInfo: {
        title: '社交圈'
    },
    data() {
        return {
            activeTab: "followers",
            followers: {
                query: {
                    page: 1,
                    pageSize: 9,
                    pageNumber: 1
                },
                loading: false,
                list: [
                  {
                    id: 1,
                    name: "lililiwj",
                    htmlUrl: "https://github.com/lililiwj",
                    avatarUrl: "https://avatars1.githubusercontent.com/u/38744589?v=4"
                  },
                  {
                    id: 2,
                    name: "shuanghewuyanzu",
                    htmlUrl: "https://github.com/shuanghewuyanzu",
                    avatarUrl: "https://avatars1.githubusercontent.com/u/38081886?v=4"
                  },
                  {
                    id: 3,
                    name: "royalosyin",
                    htmlUrl: "https://github.com/royalosyin",
                    avatarUrl: "https://avatars1.githubusercontent.com/u/18584202?v=4"
                  }
                ]
            },
            following: {
                query: {
                    page: 1,
                    pageSize: 9,
                    pageNumber: 1
                },
                loading: false,
                list: [
                  {
                    id: 1,
                    name: "kazupon",
                    htmlUrl: "https://github.com/kazupon",
                    avatarUrl: "https://avatars1.githubusercontent.com/u/72989?v=4"
                  },
                  {
                    id: 2,
                    name: "Jinjiang",
                    htmlUrl: "https://github.com/Jinjiang",
                    avatarUrl: "https://avatars1.githubusercontent.com/u/206848?v=4"
                  },
                  {
                    id: 3,
                    name: "Justineo",
                    htmlUrl: "https://github.com/Justineo",
                    avatarUrl: "https://avatars2.githubusercontent.com/u/1726061?v=4"
                  },
                ]
            },
        }
    },
    computed: {
        
    },
    mounted() {
        this.onSelect()
    },
    methods: {
      onSelect() {
                
      },
    }
}
</script>

<style>

</style>