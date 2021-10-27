<template>

<div v-for="prod in prods" :key="prod.id">
        {{prod.title}}
        <img :src="prod.image" width="100">
    </div>
    
</template>

<script>
import { onMounted, ref } from 'vue'
import axios from 'axios'
export default {
    name:"ItemCard",
    setup() {
        const prods = ref([]);

        onMounted ( async () =>{
            const {data} = await axios.get(`product`)
            prods.value = [data.data[0], data.data[1]]

        });

        return{
            prods
        }
        
    },
}
</script>