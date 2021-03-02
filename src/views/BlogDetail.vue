<template>
  <div>
    <Header></Header>

    <div class="mblog">
      <h2>
        {{ blog.title }}
      </h2>
      <el-link icon="el-icon-edit" v-if="onBlog">
        <router-link :to="{ name: 'BlogEdit', params: { blogId: blog.id } }">
          编辑
        </router-link>
      </el-link>
      <el-divider></el-divider>
      <div class="markdown-body" v-html="blog.content"></div>
    </div>
  </div>
</template>

<script>
import Header from "../components/Header.vue";
import "github-markdown-css/github-markdown.css";
export default {
  name: "BlogDetail.vue",
  components: { Header },
  data() {
    return {
      blog: {
        id: "",
        title: "标题",
        content: "内容",
      },
      onBlog: false,
    };
  },
  created() {
    const blogId = this.$route.params.blogId;
    console.log(blogId);
    const _this = this;
    this.$axios.get("/blog/" + blogId).then((res) => {
      const blog = res.data.data;
      _this.blog.id = blog.id;
      _this.blog.title = blog.title;
      _this.blog.description = blog.description;

      var MarkdownIt = require("markdown-it");
      var md = new MarkdownIt();

      var result = md.render(blog.content);
      _this.blog.content = result;

      _this.onBlog = blog.userId === _this.$store.getters.getUser.id;
    });
  },
};
</script>

<style>
.mblog {
  box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
  width: 100%;
  min-height: 700px;
}
</style>