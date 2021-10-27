<template>
    <header class="navbar navbar-dark sticky-top bg-dark flex-md-nowrap p-0 shadow">
        <a class="navbar-brand col-md-3 col-lg-2 me-0 px-3" href="#">Company name</a>
        
        <nav class ="my-2 my-md-0 mr-md-3">
            <router-link  class="p-2 text-white text-decoration-none" to="/profile">{{name}}</router-link>
            <router-link to="/login" class="p-2 text-white text-decoration-none" @click="logout" >Sign out</router-link>
            
        </nav>
        
    </header>
</template>

<script lang = 'ts'>
import axios from 'axios'
import {  computed, ref, watch } from 'vue'
import { useStore } from 'vuex';

export default {
    name:'Nav',
    setup(){

      const name = ref('');

      const store = useStore()
      
      const user =  computed(()=> store.state.User.user);

      
      watch(user, ()=> {
          name.value = user.value.first_name + ' ' + user.value.last_name
      })
      

      const logout = async () =>{
          
          const mess =await axios.get('logout');
          console.log(mess)


      }

      return{
          name,
          logout
      }
    }
}
</script>
