<template>
<div class="table-responsive">
        <table class="table table-striped table-sm">
          <thead>
            <tr>
              <th scope="col">Image</th>
              <th scope="col">Title</th>
              <th scope="col">Description</th>
              <th scope="col">Price</th>
              <th scope="col">Action</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="cart_element in cart_elements" :key="cart_element.product.id" >
              <td @click="increase(cart_element.product.id)"><img :src="cart_element.product.image" width="50" ></td>
              <td @click="increase(cart_element.product.id)">{{cart_element.product.title}}</td>
              <td @click="increase(cart_element.product.id)">{{cart_element.product.description}}</td>
              <td @click="increase(cart_element.product.id)">{{cart_element.product.price}}</td>
              <td>
                  <div class="mb-3 btn-group">
                        <input style="width:100px" type="number" class="form-control" min="0"  v-model="cart_element.count"  @change="check(cart_element.product.id, $event.target.value)">
                        <a href="javascript:void(0)" class="btn btn-sm btn-outline-secondary" @click="reset(cart_element.product.id)">X</a> 
                  </div>
              </td>
            </tr>
            
          </tbody>
        </table>
</div>
<div style="width:100%; margin-bottom : 30px; text-align: center;">
<a style="margin:auto; width:30%" href="javascript:void(0)" @click="submit" class="btn  btn-success ">Buy</a>

</div>

<Paginator :lastpage="lastpage" @page-changed="load_prods($event)"/>
    
</template>


<script lang="ts">
import { computed, onMounted, ref } from 'vue'
import axios from 'axios'
import { Product } from '@/models/product'
import Paginator from '@/components/Paginator.vue'
import { useStore } from 'vuex'
export default {
    name:"Cart",
    components:{Paginator},
    setup() {

        const selected_products = ref([] as any)
        
        const lastpage = ref(0)

        const store = useStore()

        const user = computed(()=> store.state.User.user);

        const cart_elements = ref([] as any)

        const load_prods = async (page=1) =>{
            
            const {data} = await axios.get(`product?page=${page}`)
            
            lastpage.value = data.meta.lastPage;

            cart_elements.value=  data.data.map((prod:Product)=>
                {
                  const cart = selected_products.value.find((c:any) => c.product.id===prod.id)
                  let count = 0
                  if(cart){
                    count = cart.count

                  }
                  return {product:prod,count:count}}
            )
        }

        onMounted(load_prods)

        const reset= (id:string) =>{

            const elem = cart_elements.value.find((c:any) => c.product.id===id);
            selected_products.value = selected_products.value.filter((pr:any) => pr.product.id !== elem.product.id)
            elem.count=0
        }

        const increase= (id:number) =>{
            const elem = cart_elements.value.find((c:any) => c.product.id===id);
            if(elem.count === 0 ){
              selected_products.value = [...selected_products.value, elem]
            }
            elem.count+=1
            const prod = selected_products.value.find((c:any) => c.product.id===id);
            prod.count=elem.count
        }

        const check = (id:number, new_value:number)=>{
            const elem = cart_elements.value.find((c:any) => c.product.id===id);
            if(Number(new_value) === 0 ){
              selected_products.value = selected_products.value.filter((pr:any) => pr.product.id !== id)
            }
            else{
              const prod = selected_products.value.find((c:any) => c.product.id===id);
              if(prod){
                prod.count=Number(new_value)

              }
              else{
                selected_products.value = [...selected_products.value, elem]
                
              }
            }
        }

        const submit = async () =>{
          if(selected_products.value.length){

            const order_post = {
              first_name : user.value.first_name,
              last_name : user.value.last_name,
              email:user.value.email,
              order_items:[]
            }

            const {data} = await axios.post('orders', order_post)

            const order = data;

            let new_items_ids = [] as any

            for(let cart_item of selected_products.value){
              const order_item_post ={
                product_title:cart_item.product.title,
                price : cart_item.product.price,
                quantity: cart_item.count,
                order_id:order.id
              }
              
              const {data} = await axios.post('order_items',order_item_post)
              const item = data;
              new_items_ids = [item.id, ...new_items_ids]

            }

            await axios.put(`orders/${order.id}`,{order_items:new_items_ids})

            selected_products.value = []

            load_prods()
            
          }

        }


        return{
            
            reset,
            cart_elements,
            increase,
            load_prods,
            lastpage,
            check,
            submit

        }
        
    },
}
</script>