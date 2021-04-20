<template>
  <div>
    <div>
      <el-dialog
        :visible.sync="dialogVisible"
        fullscreen
        :before-close="handleClose"
      >
        <div @click="submit">新增</div>
        <div class="richText">
          <el-form
            :model="ruleForm"
            ref="ruleForm"
            label-width="100px"
            class="demo-ruleForm"
          >
            <el-form-item label="标题" prop="name">
              <el-input v-model="ruleForm.name"></el-input>
            </el-form-item>

            <el-form-item label="封面">
              <el-upload
                class="avatar-uploader"
                action="/vue-element-admin/media/create/"
                :show-file-list="false"
                :on-success="handleAvatarSuccess"
                :before-upload="beforeAvatarUpload"
              >
                <img v-if="imageUrl" :src="imageUrl" class="avatar" />
                <i v-else class="el-icon-plus avatar-uploader-icon"></i>
              </el-upload>
            </el-form-item>

            <el-form-item label="试看内容" prop="delivery" required>
              <RichText></RichText>
            </el-form-item>
            <el-form-item label="课程内容" prop="delivery" required>
              <RichText></RichText>
            </el-form-item>
            <el-form-item label="课程价格" prop="type">
              <el-input-number
                v-model="num"
                @change="handleChange"
                :min="1"
                :max="10"
                label="描述文字"
              ></el-input-number>
            </el-form-item>
            <el-form-item label="状态">
                <el-radio  v-model="ruleForm.radio" label="1">上架</el-radio>
                <el-radio  v-model="ruleForm.radio" label="0">下架</el-radio>
            </el-form-item>
          </el-form>
        </div>
      </el-dialog>
    </div>
  </div>
</template>

<script>
import RichText from "@/components/Tinymce";
import { fetchList, createMedia, updateMedia, deleteMedia } from "@/api/media";
export default {
  name: "Edit",
  data() {
    return {
      dialogVisible: false,
      num: 1,
      imageUrl: "",
      ruleForm: {
        name: "",
        delivery: "",
        type: [],
        radio: "1",
        desc: ""
      }
    };
  },
  methods: {
    handleClose() {
      this.dialogVisible = false;
    },
    parentHandleclick() {
      this.dialogVisible = true;
    },
    handleAvatarSuccess() {},
    beforeAvatarUpload() {},
    handleChange() {},
    async submit() {
      let res = await createMedia({
        title: this.ruleForm.name,
        cover: "http://dummyimage.com/200x100",
        content: this.ruleForm.delivery,
        try: this.ruleForm.delivery,
        price: this.num,
        status: this.resource
      });
      if (res.data === "success") {
        this.$confirm("是否继续?", "提示", {
          confirmButtonText: "确定",
          cancelButtonText: "取消",
          type: "warning"
        })
          .then(() => {
            this.$message({
              type: "success",
              message: "添加成功!"
            });
          })
          .catch(() => {
            this.$message({
              type: "info",
              message: "已取消"
            });
          });
      }
    }
  },

  components: {
    RichText
  }
};
</script>
<style scoped>
.avatar-uploader .el-upload {
  border: 1px dashed red;
  border-radius: 6px;
  cursor: pointer;
  position: relative;
  overflow: hidden;
}
.avatar-uploader .el-upload:hover {
  border-color: #409eff;
}
.avatar-uploader-icon {
  font-size: 28px;
  color: #8c939d;
  width: 178px;
  height: 178px;
  line-height: 178px;
  text-align: center;
}
.avatar {
  width: 178px;
  height: 178px;
  display: block;
}
.richText {
  width: 800px;
}
</style>
