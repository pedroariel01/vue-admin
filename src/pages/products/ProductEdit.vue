<template>
    <form @submit.prevent="submit">
        <div class="mb-3">
            <label >Title</label>
            <input  v-model="form.title" class="form-control" name="title">
        </div>
        <div class="mb-3">
            <label >Description</label>
            <textarea v-model="form.description" class="form-control" name="description"></textarea>
        </div>
        <div class="mb-3">
            <label >Image</label>
            <div class="input-group">
                <input v-model="form.image" class="form-control" name="image">
                <ImageUpload @uploaded="form.image=$event"/>
                
            </div>
        </div>
        <div class="mb-3">
            <label >Price</label>
            <input v-model="form.price" class="form-control" name="price">
        </div>

        <button class="btn btn-sm btn-outline-secondary" > Save</button>
    </form>
</template>

<script lang="ts">
import { onMounted, reactive } from 'vue'
import axios from 'axios';
import { useRoute, useRouter } from 'vue-router';
import ImageUpload from '@/components/ImageUpload.vue';

export default {
  components: { ImageUpload },
    name:"ProductEdit",
    setup() {

        


        const form =reactive({
                title:'',
                description:'',
                image:'',
                price:''

            }
        );

        const route = useRoute()

        onMounted( async ()=>{
            const {data} = await axios.get(`product/${route.params.id}`)
            form.title = data.title;
            form.description = data.description;
            form.image = data.image;
            form.price = data.price;

        })


        const router = useRouter()

        const submit = async () => {
            await axios.put(`product/${route.params.id}`,form)

            router.push('/products')

        }


        return{
            form,
            submit
        }        
    },
}
</script>
