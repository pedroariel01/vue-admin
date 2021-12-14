<template>
<Nav />


<div class="main__container">
  
    <div class="main__menu">
      <Menu/>

    </div>
    

    <main class="main__content"><div class="chartjs-size-monitor"><div class="chartjs-size-monitor-expand"><div class=""></div></div><div class="chartjs-size-monitor-shrink"><div class=""></div></div></div>
      <router-view/>
      
    </main>
  
</div>
    
</template>

<script lang='ts'>

import Nav from "@/components/Nav.vue"
  import Menu from '@/components/Menu.vue'
import axios from 'axios'
import { onMounted } from 'vue'
import { useRouter } from 'vue-router'
import { useStore } from 'vuex'


  export default {
      name:"Wrapper",
    components: {Nav,Menu},
    setup(){
      const router = useRouter();

      const store = useStore();
      
      
        onMounted(async () => {
          try{

            const {data} = await axios.get('user');

            await store.dispatch('User/setUser',data);

          }
          catch (e){
           await router.push('/login');

          }
          
        });
      
       
    }
    
  }

</script>

<style scoped>
.main__container{
  padding:0 12px ;
  display: grid;
  grid-template-columns: 15vw 85vw;
  grid-gap: 10px;
}

.main__menu{
  grid-column: 1/2;
}

.main__content{
  grid-column:2/3;
}
</style>