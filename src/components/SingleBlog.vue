<template>
    <div id="single-blog">
        <h1>{{blog.title}}</h1>
        <article>{{blog.body}}</article>
        <button @click="deleteSingleBlog">删除</button>
        <router-link :to="'/edit/' + id">编辑</router-link>
    </div>
</template>

<script>
export default {
    name:"single-blog",
    data () {
        return {
            id:this.$route.params.id,
            blog:{}
        }
    },
    created() {
        this.$http.get('https://jsonplaceholder.typicode.com/posts/' + this.id)
            .then(function(data){
                // console.log(data);
                this.blog = data.body;
            })
    },
    methods: {
        deleteSingleBlog(){
            this.$http.delete("https://jsonplaceholder.typicode.com/posts/" + this.id)
                .then(function (data) {

                  })
        }
    },
}
</script>

<style scoped>
    #single-blog{
        max-width: 960px;
        margin: 0 auto;
        padding: 20px;
        background: #eee;
        border: 1px dotted #aaa;
    }
</style>

