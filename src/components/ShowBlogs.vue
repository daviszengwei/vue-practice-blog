<template>
  <div v-theme:column="'wide'" id="show-blogs">
      <h1>博客总览</h1>
      <input type="text" placeholder="搜索" v-model="search">
      <div v-for="blog in filterdBlogs" :key="blog.id" class="single-blog">
          <router-link v-bind:to="'/blog/'+ blog.id">
              <h2 v-rainbow>{{blog.title | to-uppercase}}</h2>
            </router-link>
          <article>{{blog.body | snippet}}</article>
      </div>
  </div>
</template>

<script>

export default {
  name: 'show-blogs',
    data(){
        return{
            blogs:[],
            search:""
        }
    },
    created(){
        this.$http.get("https://jsonplaceholder.typicode.com/posts")
        .then(function(data){
            // console.log(data)
            this.blogs = data.body.slice(0,10);
        })
    },
    computed:{
        filterdBlogs:function(){
            return this.blogs.filter((blog)=>{
                return blog.title.match(this.search);
            })
        }
    },
    //局部的过滤器
    filters:{
        "to-uppercase":function(value){
            return  value.toUpperCase();
        },
        snippet(value){
            return value.slice(0,100) + "..."
        }
    },
    //局部的自定义指令
    directives:{
        'rainbow':{
            bind(el,binding,vnode){
                el.style.color = "#" + Math.random().toString(16).slice(2,8);
            }
        }
    }
}
</script>

<style>
    #show-blogs{
        max-width: 800px;
        margin: 0 auto;
    }
    .single-blog{
        padding: 20px;
        margin: 20px 0;
        box-sizing: border-box;
        background: #eee;
        border:1px dotted #aaa
    }
    #show-blogs a{
        color: #444;
        text-decoration: none;

    }
    input[type="text"]{
        padding: 8px;
        width: 100%;
        box-sizing: border-box;
    }
</style>
