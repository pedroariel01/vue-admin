<template>

    <div class="pt-3 pb-2 mb-3 border-bottom">
      <router-link to="/products/create" class="btn btn-sm btn-outline-secondary">Add</router-link>
    
    </div>
    <div class="table-responsive">
        <table class="table table-striped table-sm">
          <thead>
            <tr>
              <th scope="col">#</th>
              <th scope="col">Image</th>
              <th scope="col">Title</th>
              <th scope="col">Description</th>
              <th scope="col">Price</th>
              <th scope="col">Action</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="prod in products" :key="prod.id">
              <td>{{prod.id}}</td>
              <td><img :src="prod.image" width="50"></td>
              <td>{{prod.title}}</td>
              <td>{{prod.description}}</td>
              <td>{{prod.price}}</td>
              <td>
                <div class="btn-group mr-2">
                  <router-link :to="`/products/${prod.id}/edit`" class="btn btn-sm btn-outline-secondary">Edit</router-link>
    
                  <a href="javascript:void(0)" class="btn btn-sm btn-outline-secondary" @click="del(prod.id)">Delete</a> 
               </div>
              </td>
            </tr>
            
          </tbody>
        </table>
      </div>

      <Paginator :lastpage="lastpage" @page-changed="load($event)"/>

      
</template>

<script lang = "ts">
import { onMounted, ref, watch } from 'vue'
import axios from 'axios'
import {Product} from '@/models/product'
import Paginator from '@/components/Paginator.vue'

export default {
    name:"Products",
    components:{Paginator},
    setup() {

        const products = ref([])

        

        const lastpage = ref(0)

         const load = async (page=1)=>{
            const {data} = await axios.get(`product?page=${page}`)
            products.value = data.data
            lastpage.value = data.meta.lastPage;
        }

        onMounted(load)

        

        const del = async (id:number) =>{
            if(confirm('Are you sure')){
            await axios.delete(`product/${id}`);
            products.value = products.value.filter((pr: Product)=> {
                pr.id !== id;
            })
            } 

        }

        

        return{
            products,
            del,
            lastpage,
            load

        }
    },
}
</script>