<template>
<h3> Account Information</h3>
    <form @submit.prevent="info_submit" >
        <div class="mb-3">
            <label >First Name</label>
            <input v-model="info.first_name" class="form-control" name="first_name"></div>
        <div class="mb-3">
            <label >Last Name</label>
            <input v-model="info.last_name" class="form-control" name="last_name"></div>
        <div class="mb-3">
            <label >Email</label>
            <input v-model="info.email" class="form-control" name="email"></div>

        <button class="btn btn-outline-secondary">Save</button>
    </form>


<h3 class="mt-4">Change Password</h3>
    <form @submit.prevent="pass_submit">
        <div class="mb-3">
            <label >Password</label>
            <input v-model="pass.password" type="password" class="form-control" name="password"></div>
        <div class="mb-3">
            <label >Password Confirm</label>
            <input v-model="pass.confirm_password" type="password" class="form-control" name="password_confirm"></div>
        
        <button class="btn btn-outline-secondary">Save</button>

    </form>


</template>

<script lang="ts">
import { computed, onMounted, reactive, watch } from 'vue'
import axios from 'axios'
import { useStore } from 'vuex'
export default {
    name:"Profile",
    setup() {

        const info = reactive({
            first_name:"",
            last_name:"",
            email:""
        })

        const pass  = reactive({
            password:"",
            confirm_password:""
        })

        
      const store = useStore()
      
      const user =  computed(()=> store.state.User.user);

      

      const loadInfo = ()=> {
          info.first_name = user.value.first_name;
          info.last_name = user.value.last_name;
          info.email = user.value.email;
      }

      watch(user, loadInfo)

      onMounted(loadInfo)

        

        const info_submit = async ()=>{
            const {data} = await axios.put('users/info',info)

            await store.dispatch('User/setUser',data)
        }

        const pass_submit = async ()=>{
            await axios.put('users/info',pass)
            

        }

        return {
            info,
            pass,
            info_submit,
            pass_submit

        }
        
    },
}
</script>