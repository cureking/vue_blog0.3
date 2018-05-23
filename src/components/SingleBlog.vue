<template>
  <div id="single-blog">
    <h1>{{blog.title}}</h1>
    <article>{{blog.content}}</article>
    <p>作者：{{blog.author}}</p>
    <ul>
      <li v-for="category in blog.categorates">
        {{category}}
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'single-blog',
  data () {
    return {
      id: this.$route.params.id,
      blog: {}
    }
  },
  created(){
//    this.$http.get( "https://blog1-6.firebaseio.com/posts/" + this.id +".json" )
    axios.get( "/posts/" + this.id +".json" )
      .then( ( data )=>
      {
        return data.data;
      } )
      .then(( data )=>
      {
        this.blog = data;
      })
  }
}
</script>
<style scoped>
  #single-blog{
    max-width: 960px;
    margin:0 auto;
    padding:20px;
    background: #eee;
    border:1px dotted #aaa;
  }
</style>
