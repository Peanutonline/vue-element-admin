<template>
 <div>
     <Header :ti='ti'></Header>
 </div>
</template>
<script>
import Header from "../../components/header/header"
export default {
  name: "Video",
  data(){
      return{
          ti:'新增视频',
      }
  },
  components:{
      Header
  }
};
</script>
<style scoped>

</style>
