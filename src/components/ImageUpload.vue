<template>
    <label class=" btn btn-primary">
                    Upload <input type="file" hidden @change="upload($event.target.files)">
                </label>
</template>

<script>
import axios from 'axios';

export default {
    name:"ImageUpload",
    emits:['uploaded'],
    setup(_, context) {

        const upload = async (files)=>{
            if(files ===null) return;

            const file = files.item(0)

            const form_data = new FormData()

            form_data.append('image',file)

            const {data}= await axios.post('upload', form_data)

            context.emit('uploaded',data.url);

        }

        return{
            upload

        }
        
    },
}
</script>