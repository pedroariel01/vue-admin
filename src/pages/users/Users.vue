<template>

    <div class="pt-3 pb-2 mb-3 border-bottom">
      <router-link to="/users/create" class="btn btn-sm btn-outline-secondary">Add</router-link>
    
    </div>
    <div class="table-responsive">
        <table class="table table-striped table-sm">
          <thead>
            <tr>
              <th scope="col">#</th>
              <th scope="col">Name</th>
              <th scope="col">Email</th>
              <th scope="col">Role</th>
              <th scope="col">Action</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="user in users" :key="user.id">
              <td>{{user.id}}</td>
              <td>{{user.first_name}} {{user.last_name}}</td>
              <td>{{user.email}}</td>
              <td>{{user.role.name}}</td>
              <td>
                <div class="btn-group mr-2">
                  <router-link :to="`/users/${user.id}/edit`" class="btn btn-sm btn-outline-secondary">Edit</router-link>
    
                  <a href="javascript:void(0)" class="btn btn-sm btn-outline-secondary" @click="del(user.id)">Delete</a>
                </div>
              </td>
            </tr>
            
          </tbody>
        </table>
      </div>

    <Paginator :lastpage="lastpage" @page-changed="load($event)"/>

</template>


<script lang='ts'>
import { onMounted, ref, watch } from 'vue'
import axios from 'axios'
import {User} from "@/models/user"
import Paginator from '@/components/Paginator.vue'


export default {
  
    name:"Users",
    components:{Paginator},
    setup(){

      const users = ref([])
      

      const lastpage = ref(0)

      const load = async (page=1) => {
        const {data}= await axios.get(`users?page=${page}`)

        users.value = data.data;

        lastpage.value = data.meta.lastPage;
      }

      onMounted( load);
      

      const del = async (id:number) =>{
        if(confirm('Are you sure')){
          await axios.delete(`users/${id}`);
          users.value = users.value.filter((us: User)=> {
            us.id !== id;
          })
        } 

      }

      return{
        users,
        load,
        lastpage,
        del
      }

    }

}
</script>