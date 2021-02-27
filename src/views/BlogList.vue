<template>
  <div class="mcontaner">
    <Header></Header>
    <div class="block">
      <el-timeline>
        <el-timeline-item
          placement="top"
          :timestamp="blog.created"
          v-for="(blog, index) in blogList"
          :key="index"
        >
          <el-card>
            <h4>
              <router-link
                :to="{ name: 'BlogDetail', params: { blogId: blog.id } }"
              >
                {{ blog.title }}
              </router-link>
            </h4>
            <p>{{ blog.description }}</p>
          </el-card>
        </el-timeline-item>
      </el-timeline>

      <el-pagination
        class="mpage"
        background
        layout="prev, pager, next"
        :total="1000"
      >
      </el-pagination>
    </div>
  </div>
</template>

<script>
import Header from "../components/Header";

export default {
  name: "BlogList.vue",
  components: { Header },
  data() {
    return {
      blogList: {},
      currentPage: 1,
      total: 0,
      pageSize: 5,
    };
  },
  methods: {
    page(currentPage) {
      const _this = this;
      _this.$axios
        .get("/blog/blogList?currentPage=" + currentPage)
        .then((res) => {
          console.log(res);
          _this.blogList = res.data.data.records;
          _this.currentPage = res.data.data.currentPage;
          _this.total = res.data.data.total;
          _this.pageSize = res.data.data.pageSize;
        });
    },
  },
  created() {
    this.page(1);
  },
};
</script>

<style>
.mpage {
  margin: 0 auto;
}
</style>