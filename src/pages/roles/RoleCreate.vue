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
                <input class="form-check-input" type="checkbox" :value="perm.id" @change="select(perm.id, $event.target.checked)">
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
import { useRouter } from 'vue-router'

export default {
    name:'RoleCreate',
    setup() {

        const {push} = useRouter()

        const from_data  = reactive<any>({
            name:'',
            permissions: []
        })

        const all_perms =  ref<any>([])

        onMounted( async ()=>{
            const {data} = await axios.get('permissions')
            all_perms.value = data;

        });

        const select = (id:number, checked:boolean) => {
            if(checked){
                from_data.permissions = [...from_data.permissions, id];
                return;
            }

            from_data.permissions = from_data.permissions.filter( (p:Permission )=> p.id !== id);

        }

        const submit = async () => {
            await axios.post('roles',from_data)

            push('/roles')

        }


        return {
            
            select,
            submit,
            all_perms,
            from_data

            


        }
    },
}
</script>