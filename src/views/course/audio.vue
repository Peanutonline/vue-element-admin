<template>
 <div>
     <Header :ti='ti'></Header>
 </div>
</template>
<script>
import Header from "../../components/header/header"
export default {
  name: "Audio",
  data(){
      return{
          ti:'新增音频',
      }
  },
  components:{
      Header
  }
};
</script>
<style scoped>

</style>
