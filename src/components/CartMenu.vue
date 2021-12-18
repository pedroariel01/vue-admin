<template>
    <div class="buy__nav">
  
        <svg class="buy__layout"  version="1.1" viewBox="0 0 649.42 111.03" xmlns="http://www.w3.org/2000/svg">
        
        <defs>
            <linearGradient id="grad2" x1="0%" y1="0%" x2="100%" y2="100%">
            <stop offset="0%" style="stop-color:rgba(29,199,234);stop-opacity:0.85" />
            <stop offset="50%" style="stop-color:rgba(41,52,255);stop-opacity:0.85" />
            <stop offset="100%" style="stop-color:rgba(59,89,152) ;stop-opacity:0.85" />
            </linearGradient>
        </defs>
        <g transform="translate(173 -10)">
        <path d="m124.45 7.4579-5.3e-4 0.08992-292.6 0.48111s-11.565 45.681 18.277 43.979c157.69-8.9971 217.61 26.396 219.64 38.267 3.4921 20.459 57.115 22.913 57.115 22.913l24.831-0.0987 24.831 0.0987s53.623-2.4538 57.115-22.913c2.0263-11.872 61.945-47.264 219.64-38.267 29.842 1.7026 18.277-43.979 18.277-43.979l-292.6-0.48111-5.3e-4 -0.08992-27.262 0.04496z" fill= "url(#grad2)"/>
        </g>
        <foreignObject class='buy__area'>
            <div>
            <button class="buy__button" style="transform:scale(1)">BUY</button>
            </div>
        </foreignObject>
        
        
        </svg>
  
    </div>

    <div class="card__section">

        <div class="card__displayer">
            <div class="card__content" v-for="cart in cart_elements" :key="cart.product.id" @click="select(cart.product.id)">
                <transition name= "ripple">
                    <div v-if="selected===cart.product.id" class ="card__effect"></div>
                </transition>
                <CartCard :cart_item="cart" @add="increase($event)" @remove="reset($event)"/>
            </div>
        </div>
    </div>

    

</template>

<script lang = "ts">
import { computed, onMounted, ref } from 'vue'
import axios from 'axios'
import { Product } from '@/models/product'
import Paginator from '@/components/Paginator.vue'
import { useStore } from 'vuex'
import CartCard from '@/components/CartCard.vue';
export default {
    name:"CartMenu",
    components:{CartCard,Paginator},
    setup() {

       const selected_products = ref([] as any)
        
        const lastpage = ref(0)

        const store = useStore()

        const user = computed(()=> store.state.User.user);

        const selected = ref(0)

        const select = (id:number)=>{

            selected.value = id;
            setTimeout(()=>{selected.value=0},200)
        }

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

        const reset= (id:number) =>{

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
            submit,
            select,
            selected

        }
        
    },
}
</script>

<style>

.card__section{
    margin-top:100px;
}

.card__displayer{
    margin:5rem 3rem;
    width: 100%;
    display: grid;
  grid-template-columns: repeat(auto-fit, 17rem);
  grid-auto-rows:20rem;
  grid-auto-flow: dense;
  grid-gap: 4rem;
  

}

.card__content::after{
    content:"";
    display: inline-block;
    height: 100%;
    width:100%;
    border-radius: 14px;
    background-image:linear-gradient( to right bottom,
                                    rgba(41,52,255,0.85),
                                    rgba(29,199,234,0.85));
    top:0;
    left: 0;
    position: absolute;
    z-index : -1;
    
  }


.card__content{
    position: relative;
    width:15rem;
    height:18rem;
    perspective:1000px;
    -moz-perspective: 1000px;
    
    backface-visibility: hidden;
    transition: all .4s ease-in-out;

}

.card__content:hover{
    width: 18rem;
    height: 22rem;
    transform: scale(1.07) , rotate(0.01);
    
}


.card__content:hover>.card__wrapper{
    background-image:linear-gradient( to right bottom,
    rgba(59,89,152,0.85),
                                    rgba(41,52,255,0.85)
                                    );
    
}


.card__effect{
    display: inline-block;
    
    height: 100%;
    width:100%;
    border-radius: 14px;
    background-image:linear-gradient( to right bottom,
                                    rgba(41,52,255,0.85),
                                    rgba(29,199,234,0.85));
    top:0;
    left: 0;
    position: absolute;
    z-index : -1;
    
}

.show::after{
animation-name:ripple;
animation-duration : .4s
}

.ripple-enter-active {
  animation: ripple-in 0.5s;
}



@keyframes ripple-in {
  0%{
    opacity: 1;
    background-color: blue;
  }

  99%{
    opacity:0;
    transform: scale(1.3);
  }

  100%{
    opacity: 1;
    transform: scale(1);
  }
}

.hide::after{
    background-image:linear-gradient( to right bottom,
                                    rgba(41,52,255,0.85),
                                    rgba(29,199,234,0.85));
      opacity: 1;
}

.buy__nav{
  
   z-index:1;
   transition:all .3s;
  transform:translateY(-70px);
  position:fixed;
}

.buy__nav:hover{
    transform:translateY(0);
}

.buy__layout{
  width:95%;
  z-index:1;
  
  
}

.buy__area{
  width:100%;
  height:100%;
  z-index:100;
  
  position: relative;
  
}

.buy__button{
  margin: 0;
  position: absolute;
  width:130px;
  height:40px;
  border:none;
  top: 45%;
  left: 40%;
  transform: translate(-50%, -50%);
  background-color:rgba(41,52,255,.85);
  color:white;
  border-radius: 1000px;
  transition:all .4s;
  

}

.buy__button:hover{
    background-color:rgba(59,89,152,0.85);
    outline: none;
    animation: pulsate 1s infinite;
}

@keyframes pulsate {

  0%{

    transform:  scale(1);
    box-shadow: none;

  }

  50%{

    transform:  scale(1.05);
    box-shadow: 0 5px 9px rgba(0,0,0,.2);
  }

  100%{

    transform: scale(1);
    box-shadow: none;

  }

}

::selection{
  background-color: rgba(41,52,255,.85);
  color: white;

}


</style>