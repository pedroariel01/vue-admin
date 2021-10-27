<template>
    <nav >
        <ul class="pagination">
          <li class="page-item">
            <a class="page-link" href="javascript:void(0)" @click="previous"> Previous
            </a>
          </li>
          <li class="page-item">
            <a class="page-link" href="javascript:void(0)" @click="next"> Next
            </a>
          </li>
        </ul>
      </nav>
    
</template>

<script lang="ts">
import { ref, SetupContext, watch } from 'vue';
export default {
    name:"Paginator",
    props:{
        lastpage:Number
    },
    emits:['page-changed'],
    setup(props:any, context:SetupContext) {
        const page = ref(1)

        watch(page,()=>{
            
            context.emit('page-changed',page.value);

        })

        const next =  () =>{
        
        
        if(page.value < props.lastpage){
          page.value++;
          
        }

      }


      const previous =  () =>{
         if( page.value > 1) {
          page.value--;
          
         }
      }

        return {
            next,
            previous

        }
        
    },
}
</script>