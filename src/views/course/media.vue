<template>
  <div>
    <!-- 头部 -->
    <Header :ti="ti"></Header>
    <!-- 主题部分 -->

    <div class="container">
      <el-table :data="tableData" border style="width: 100%">
        <el-table-column
          label="ID"
          prop="id"
          width="100"
          sortable
          header-align="center"
          align="center"
        >
        </el-table-column>
        <el-table-column label="图文内容" prop="text" width="1100">
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
        <el-table-column
          label="订阅量"
          prop="sub_count"
          width="100"
          header-align="center"
          align="center"
        >
        </el-table-column>
        <el-table-column
          label="状态"
          width="100"
          header-align="center"
          align="center"
        >
          <template slot-scope="scope">
            <el-tag type="success" v-show="scope.row.status === 1">
              已上架
            </el-tag>
            <el-tag type="danger" v-show="scope.row.status === 0">
              已下架
            </el-tag>
          </template>
        </el-table-column>
        <el-table-column
          label="创建时间"
          prop="created_time"
          width="200"
          header-align="center"
          align="center"
        >
        </el-table-column>
        <el-table-column
          label="操作"
          prop="todo"
          width="300"
          header-align="center"
          align="center"
        >
          <template slot-scope="scope">
            <el-button
              size="mini"
              type="primary"
              @click="handleEdit(scope.row.status, scope.row)"
              >编辑</el-button
            >
            <el-button
              size="mini"
              :type="scope.row.status === 1 ? 'success' : ''"
              @click="handleUp(scope.row.status, scope.row)"
              >{{ scope.row.status === 1 ? "下架" : "上架" }}</el-button
            >
            <el-button
              size="mini"
              type="danger"
              @click="handleDelete(scope.row)"
              >删除</el-button
            >
          </template>
        </el-table-column>
      </el-table>
    </div>
    <div>
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
    <Editing 
    v-show="showDialog" 
    ref="myChild"
    ></Editing>
  </div>
  
</template>
<script>
import Header from "../../components/header/header";
import Edit from "../../components/Edit/Edit"
import Editing from "../../components/Editing/Editing"
import { fetchList, createMedia, updateMedia, deleteMedia } from "@/api/media";
export default {
  name: "Media",
  data() {
    return {
      ti: "新增图文",
      showDialog:false,
      tableData: [],
      pageNum: 1, //当前是第几页
      pageSize: 20, //每页多少条数据
      total: 0
    };
  },
  mounted() {
    this.getList();
  },
  methods: {
    async getList(a) {
      let res = await fetchList(a);
      console.log(res)
      this.tableData = res.data.items;
      this.total = res.data.total;
    },
    handleSizeChange(val) {
      this.pageSize = val;
      this.getList({
        page: this.pageNum,
        limit: this.pageSize
      });
    },
    handleCurrentChange(val) {
      this.pageNum = val;
      this.getList({
        page: this.pageNum,
        limit: this.pageSize
      });
    },
    async handleUp(status, text) {
      let res = await updateMedia({id:text.id});
      console.log(res);
      if (res.data == "success") {
        if (status == 1) {
          text.status = 0;
        } else if (status == 0) {
          text.status = 1;
        }
        this.$message({
          message:'操作success',
          type:'success'
        })
      }else{
        this.$message('操作失败')
      }
    },
    async handleDelete(text){
      console.log(text)
      let res = await deleteMedia()
      console.log(res)
      if(res.data=="success"){
        this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$message({
            type: 'success',
            message: '删除成功!'
          });
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消删除'
          });          
        });
      }
    },
   handleEdit(status,text){
     this.showDialog = true
      this.$refs.myChild.parentHandleclick(text)
    }
  },
  components: {
    Header,Edit,Editing
  }
};
</script>
<style scoped>
.container {
  padding: 20px;
}

.all {
  display: flex;
}
.all img {
  width: 200px;
  height: 100px;
}
.foot {
  margin-left: 10px;
}
</style>
