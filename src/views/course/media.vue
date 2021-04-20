<template>
  <div>
    <!-- 头部 -->
    <Header :ti="ti"></Header>
    <!-- 主题部分 -->

    <div class="container">
      <el-table :data="tableData" border style="width: 100%">
        <el-table-column label="ID" prop="id" width="100" sortable header-align="center" align="center"> 
        </el-table-column>
        <el-table-column label="图文内容" prop="text" width="1230">
          <template slot-scope="scope">
            <div class="all">
              <img :src="scope.row.cover" alt="" />
              <div class="foot">
                <p>{{ scope.row.title }}</p>
                <p style="color:red">${{ scope.row.price }}</p>
              </div>
            </div>
          </template>
        </el-table-column>
        <el-table-column label="订阅量" prop="sub_count" width="100" header-align="center" align="center"> </el-table-column>
        <el-table-column label="状态"  width="100" header-align="center" align="center"> 
          <template slot-scope="scope">
            <el-tag type="success" v-show="scope.row.status === 1">
              已上架
            </el-tag>
            <el-tag type="danger" v-show="scope.row.status === 0">
              已下架
            </el-tag>
          </template>
        </el-table-column>
        <el-table-column label="创建时间" prop="created_time" width="200" header-align="center" align="center"> </el-table-column>
        <el-table-column label="操作" prop="todo" width="300" header-align="center" align="center">
          <template slot-scope="scope">
            <el-button
              size="mini"
              type="primary"
              @click="handleEdit(scope.$index, scope.row)"
              >编辑</el-button
            >
            <el-button
              size="mini"
              type="success"
              @click="handleEdit(scope.$index, scope.row)"
              >上架</el-button
            >
            <el-button
              size="mini"
              type="danger"
              @click="handleDelete(scope.$index, scope.row)"
              >删除</el-button
            >
          </template>
        </el-table-column>
      </el-table>
    </div>
  <div class="pagination-container">
        <el-pagination
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
          :current-page="pageNum"
          :page-size="pageSize"
          layout="total, sizes, prev, pager, next, jumper"
          :total="total"
        >
        </el-pagination>
  </div>
  </div>
</template>
<script>
import Header from "../../components/header/header";
import { fetchList, createMedia, updateMedia, deleteMedia } from "@/api/media";
export default {
  name: "Media",
  data() {
    return {
      ti: "新增图文",
      tableData: [],
      pageNum:1, //当前是第几页
      pageSize:20,//每页多少条数据
      total:""
    };
  },
  mounted() {
    this.getList();
  },
  methods: {
    async getList(a,b) {
      let res = await fetchList(a,b);
      console.log(res);
      this.tableData = res.data.items;
      this.total = res.data.total
    },
    handleSizeChange(val) {
        this.pageSize = val
        this.getList({
          page:this.pageNum,
          limit:this.pageSize
        })
      },
      handleCurrentChange(val) {
        this.pageNum = val
        this.getList({
          page:this.pageNum,
          limit:this.pageSize
        })
      }
  },
  components: {
    Header
  }
};
</script>
<style scoped>
.container {
  padding: 20px;
}

.all{
  display: flex;
}
.all img {
  width: 200px;
  height: 100px;
}
.foot{
  margin-left: 10px;
}
</style>
