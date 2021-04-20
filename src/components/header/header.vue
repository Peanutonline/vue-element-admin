<template>
  <div>
    <div class="container">
      <div class="header">
        <el-button type="primary" icon="el-icon-edit" @click="handleAdd">{{ti}}</el-button>
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
    <Edit v-show="showDialog" ref="myChild"></Edit>
  </div>

</template>
<script>
import Edit from "../../components/Edit/Edit"
export default {
  name: "Audio",
  data(){
      return{
        showDialog:false,
        value:'已上架',
        options:[
          {
            value:1,
            label:'已上架'
          },
          {
            value:0,
            label:'已下架'
          }
        ],
        input:''
      }
  },
  props:{
      ti:String
  },
  methods:{
    handleAdd(){
      this.showDialog = true
      this.$refs.myChild.parentHandleclick()
    },
  },
  components:{
    Edit
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
