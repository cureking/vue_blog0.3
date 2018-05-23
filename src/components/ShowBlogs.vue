<template>
  <div v-theme:column="'wide'" id="show-blogs">
    <h1>博客总览</h1>
    <input type="text" v-model="search" placeholder="搜索">
    <div v-for="blog in filteredBlogs" class="single-blog">
      <router-link v-bind:to="'/blog/'+blog.id">
        <h2 v-rainbow>{{blog.title | to - uppercase}}</h2>
      </router-link>
      <article>
        {{blog.content}}
      </article>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'show-blogs',
  data () {
    return {
      blogs: [],
      search: '',
    }
  },
  created(){
//    this.$http.get( "https://blog1-6.firebaseio.com/posts.json" )
    axios.get( "/posts.json" )
      .then( function( data )
      {
          return data.data
      } )
      .then(( data )=>
      {
        let blogsArray = [];
        for( let key in data )
        {
          data[ key ].id = key;
          blogsArray.push( data[ key ] );
        }
        this.blogs = blogsArray;
      } )
  },
  computed: {
    filteredBlogs: function()
    {
      return this.blogs.filter( ( blog ) =>
      {
        return blog.title.match( this.search );
      } )
    }
  },
  //局部过滤器
  //两种写法
  filters: {
    "to - uppercase": function( value )
    {
      return value.toUpperCase()
    },
    //    toUppercase( value )
    //    {
    //      return value.toUpperCase()
    //    },
  },
  //局部自定义指令（拥有高于全局的优先级
  //  directives:{
  //  	'rainbow':{
  //  		bind(el,binding,vnode){
  //  			el.style.color="red";
  //      }
  //    }
  //  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  #show-blogs{
    max-width : 800px;
    margin    : 0 auto;
  }
  .single-blog{
    padding    : 20px;
    margin     : 20px;
    box-sizing : border-box;
    background : #eee;
    border     : 1px dotted #aaa;
  }
  #show-blogs a{
    color           : #444;
    text-decoration : none;
  }
  input[type="text"]{
    padding    : 8px;
    width      : 100%;
    box-sizing : border-box;
  }
</style>
