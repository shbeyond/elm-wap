<template>
  <div>
    
  </div>
</template>

<script>
import Goods from './goods.vue'

import axios from 'axios'

export default {
  name: 'HelloWorld',
  data () {
    return {

    }
  },
  created(){
    // this.initData();
  },  
  components:{
    'goods':Goods,
  
  },
  methods:{

    initData(){
  
    }

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
 
</style>
