<template>
  <section class="container">
    <div class="row">
       <li v-for="post in posts" :key="post._id" class="card">
           <h3>{{post.title}}</h3>
           <p>{{post.body}}</p>
          </li>
    </div>
  </section>
</template>

<script>
import Strapi from 'strapi-sdk-javascript/build/main'
const apiUrl = process.env.API_URL || 'http://localhost:1337'
const strapi = new Strapi(apiUrl)
export default {
  data() {
    return {
    }
  },
    computed: {
    posts() {
      return this.$store.getters['posts/list']
    }
  },
  async fetch ({ store, params }) {
    store.commit('posts/emptyList')
    const posts = await strapi.getEntries('posts');
    posts.forEach(p => {
       store.commit('posts/add',p)
    })
  }
}
</script>