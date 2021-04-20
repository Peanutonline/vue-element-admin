<template>
 <div>
     <Header :ti='ti'></Header>
 </div>
</template>
<script>
import Header from "../../components/header/header"
export default {
  name: "Column",
  data(){
      return{
          ti:'新增专栏',
      }
  },
  components:{
      Header
  }
};
</script>
<style scoped>

</style>
