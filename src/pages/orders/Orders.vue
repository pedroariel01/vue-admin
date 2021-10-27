<template>

<div class="pt-3 pb-2 mb-3 border-bottom">
      <a href="javascript:void(0)" @click="export_csv" class="btn btn-sm btn-outline-secondary">Export</a>
    
    </div>
    <div class="table-responsive">
        <table class="table table-sm">
          <thead>
            <tr>
              
              <th scope="col">Name</th>
              <th scope="col">Email</th>
              <th scope="col">Total</th>
              <th scope="col">Action</th>
            </tr>
          </thead>
          <tbody>
              <template v-for="order in orders" :key="order.id">
                    <tr>
                        
                        <td style="border-bottom:0; padding:.5rem">{{order.name}}</td>
                        <td style="border-bottom:0; padding:.5rem">{{order.email}}</td>
                        <td style="border-bottom:0; padding:.5rem">{{order.total}}</td>
                        <td style="border-bottom:0; padding:.5rem">
                            <div class="btn-group mr-2">
                  <a href="javascript:void(0)" class="btn btn-sm btn-outline-secondary" @click="select(order.id)">View</a> 
               </div>
                        </td>
                    </tr>
              
                    <tr>
                        <td colspan = "5">
                            <div class="overflow-hidden" :class="selected===order.id?'show':'hide'">
                                <table class="table table-sm">
                                    <thead>
                                        <tr>
                                            
                                            <th scope="col">Product Title</th>
                                            <th scope="col">Quantity</th>
                                            <th scope="col">Price</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <tr v-for="ord_item in order.order_items" :key="ord_item.id">
                                                
                                                <td style="border-bottom:0; padding:.5rem">{{ord_item.product_title}}</td>
                                                <td style="border-bottom:0; padding:.5rem">{{ord_item.quantity}}</td>
                                                <td style="border-bottom:0; padding:.5rem"> {{ord_item.price}}</td>
                                        </tr>
                                    </tbody>
                                </table>
                            </div>
                        </td>
                    </tr>
              </template>
            <!-- <tr v-for="order in orders" :key="order.id">
              
              <td>
                <div class="btn-group mr-2">
                  <router-link :to="`/orders/${order.id}/edit`" class="btn btn-sm btn-outline-secondary">Edit</router-link>
    
                  <a href="javascript:void(0)" class="btn btn-sm btn-outline-secondary" @click="del(order.id)">Delete</a> 
               </div>
              </td>
            </tr> -->
            
          </tbody>
        </table>
      </div>

      <Paginator :lastpage="lastpage" @page-changed="load($event)"/>
</template>

<script lang = "ts">
import { onMounted, ref } from 'vue'
import axios from 'axios'
import {Order} from '@/models/order'
import Paginator from '@/components/Paginator.vue'
export default {
    name:"Orders",
    components:{Paginator},
    setup() {

        const orders = ref([])
        const lastpage = ref(0)

        const selected = ref(0)


        const load = async (page=1)=> {
            const {data} = await axios.get(`orders?page=${page}`);
            orders.value=data.data;
            
            lastpage.value = data.meta.lastPage
        }

        onMounted(load)

        const del = async (id:number)=>{
            if(confirm('Are you sure')){
            await axios.delete(`orders/${id}`);
            orders.value = orders.value.filter((pr: Order)=> {
                pr.id !== id;
            })
            } 

        }

        const select = (id:number)=>{

            selected.value = selected.value !==id? id:0 ;
        }

        const export_csv = async () => {
            const {data} = await axios.post("export",{},{responseType:'blob'});
            const blob  = new Blob([data], {type:'text/csv'});
            const link = document.createElement('a');
            link.href  = window.URL.createObjectURL(data);
            link.download = 'orders.csv';
            link.click();


        }

        return {
            orders,
            del,
            lastpage,
            load,
            select,
            selected,
            export_csv
        }
        
    },
}
</script>


<style scoped>
.show{
max-height: 150px;
transition: max-height 500ms ease-in;
}

.hide{
    max-height: 0;
transition: max-height 500ms ease-out;

}
</style>