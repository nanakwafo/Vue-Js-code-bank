<template>
<div class="hello">
   <h4>file uploads</h4>
    <input style="display:none" type="file" @change="onFileSelected" ref="fileInput">
    <button @click="$refs.fileInput.click()">Pickup</button>
    <img :src="avatar" alt="">
    <button @click="upload">Upload</button>

</div>
</template>

<script>
import {
    axios
} from "axios"
export default {
    name: 'HelloWorld',
    data() {
        return {
            selectedFile: null,
            avatar: null,
        }
    },
    methods: {
        onFileSelected: function (event) {
            //Getting file name
            this.selectedFile = event.target.files[0];
            //Preview uploaded item
            let reader = new FileReader();
            reader.readAsDataURL(this.selectedFile);
            reader.onload = e => {
                this.avatar = e.target.result
            }

        },
        upload: function () {
            const fd = new FormData();
            fd.append('image', this.selectedFile, this.selectedFile.name)
            
            axios.post('api/mydata', fd, {
                onUploadProgress: function (progressEvent) {
                    console.log('Upload Progress' + Math.round(progressEvent.loaded / progressEvent.total * 100) + '%');
                }
            }).then(response => {
                console.log(response);

            });
        }
    }
}
</script>

<style scoped>

</style>
