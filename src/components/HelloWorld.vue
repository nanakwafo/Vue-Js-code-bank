<template>
<div class="hello">
    <h4>file uploads</h4>
    <input style="display:none" type="file" multiple @change="onFileSelected" ref="fileInput">
    <button @click="$refs.fileInput.click()">Pickup</button>
    <img :src="avatar" alt="">
    <button @click="upload">Upload</button>
    <p>{{loader}}</p>
    <button @click="check">Check</button>
    <ol>
        <li id="list" v-for="(Item,index) in selectedFiles" :key="Item"> {{Item}} <button @click="removeDoc(index)">remove</button></li>
    </ol>

</div>
</template>

<script>
import axios from "axios"
export default {
    name: 'HelloWorld',
    data() {
        return {
            
            selectedFiles: [],
            avatar: null,
        }
    },
    methods: {
        getdocuments:function(){
           axios.get('http://localhost/File-Upload-vue-Api/public/api/documents').
           then(function (response) {
                // handle success
                console.log(response);
            })
        },
         onFileSelected: function (event) {
           console.log(event.target.files);
          
          for(var i=0 ;i< event.target.files.length;i++){
                 let file = event.target.files[i].name;
                 this.selectedFiles.push(file);
          }
          
        // //Preview uploaded item
        // let reader = new FileReader();
        // reader.readAsDataURL(this.selectedFile);
        // reader.onload = e => {
        //     this.avatar = e.target.result
        // }

         },
        upload: function () {
           
            const formData = new FormData();
            for (var i = 0; i < this.$refs.fileInput.files.length; i++) {
                let file = this.$refs.fileInput.files[i];
               // console.log(file);
                formData.append('files[' + i + ']', file);
            }
            axios.post('http://localhost/File-Upload-vue-Api/public/api/document', formData, {
                headers: {
                    'Content-Type': 'multipart/form-data'
                },
                onUploadProgress: function (progressEvent) {
                 console.log('Upload Progress' + Math.round(progressEvent.loaded / progressEvent.total * 100) + '%');
                }
            }).then(response => {
                console.log(response);

            });

        },
        removeDoc :function(index){
           
           this.$delete(this.selectedFiles,index)
           
          
           
        }
    },
    created(){
     this.getdocuments();
    }
}
</script>

<style scoped>
#list{
    border-bottom: solid!important;
}
</style>


