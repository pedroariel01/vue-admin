<template>
    <form @submit.prevent="submit">
    <div class="mb-3 mt-3-row">
        <label class= "col-sm-2 col-form-label">Name</label>
        <div class="col-sm-10">
            <input  v-model="from_data.name" class="form-control" name="name">
        </div>
    </div>
    
    <div class="mb-3 row">
        <label class= "col-sm-2 col-form-label">Permissions</label>
        <div class="col-sm-10">
            <div class="form-check form-check-inline col-3" v-for="perm in all_perms" :key="perm.id">
                <input class="form-check-input" type="checkbox" :value="perm.id" 
                @change="select(perm.id, $event.target.checked)"
                :checked= "checked(perm.id)">
                <label class="form-check-label">{{perm.name}}</label>

            </div>
        </div>
        
    </div>

    <button class="btn btn-sm btn-outline-secondary" > Save</button>
</form>
</template>

<script lang="ts">
import { onMounted, reactive, ref } from '@vue/runtime-core'
import axios from 'axios'
import {Permission} from "@/models/permission"
import { useRoute, useRouter } from 'vue-router'

export default {
    name:'RoleEdit',
    setup() {

        const {push} = useRouter()

        const route = useRoute();

        const from_data  = reactive<any>({
            name:'',
            permissions: []
        })

        const all_perms =  ref<any>([])

        onMounted( async ()=>{
            const {data} = await axios.get('permissions')
            const response = await axios.get(`roles/${route.params.id}`);


            from_data.name = response.data.name;
            from_data.permissions = response.data.permissions.map((p:Permission)=> p.id)

            all_perms.value = data;

        });

        const select = (id:number, checked:boolean) => {
            if(checked){
                from_data.permissions = [...from_data.permissions, id];
                
                return;
            }

            from_data.permissions = from_data.permissions.filter( (p:number )=> p !== id);

        }

        const submit = async () => {
            
            await axios.put(`roles/${route.params.id}`,from_data)

            push('/roles')

        }

        const checked = (id:number) : boolean=>{
            return from_data.permissions.some((p:number) => p === id)
        }


        return {
            
            select,
            submit,
            all_perms,
            from_data,
            checked

        }
    },
}
</script>