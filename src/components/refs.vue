<template>
<div>
    <h4> Begining with refs</h4>
    <input ref="name" />

    <button v-on:click="submit"> Click me</button>
</div>
</template>

<script>
export default {
    name: "",
    data() {
          return{

          }
    },
    methods: {
       submit:function(){
            console.log(this.$refs.name.value);
       }
    },
    computed: {

    },
    created() {
       
    }
}
</script>

<style scoped>

</style>
