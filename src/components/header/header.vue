<template>
  <div>
    <div class="container">
      <div class="header">
        <el-button type="primary" icon="el-icon-edit">{{ti}}</el-button>
        <div class="right">
          <el-select v-model="value" placeholder="商品状" class="select">
            <el-option
              v-for="item in options"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            >
            </el-option>
          </el-select>
          <el-input v-model="input" placeholder="标题" class="ipt"></el-input>
          <el-button type="primary" icon="el-icon-search">搜索</el-button>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "Audio",
  data(){
      return{
      }
  },
  props:{
      ti:String
  }

};
</script>
<style scoped>
.container{
    padding: 20px;
    box-sizing: border-box;
}
.header {
  display: flex;
  justify-content: space-between;
}
.select {
  width: 100px;
  margin-right: 20px;
}
.ipt {
  width: 200px;
}
</style>
