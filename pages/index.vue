<template>
  <div>
    <!-- render data of the person -->
    <h1>{{ person.fields.name }}</h1>
    <img :src="person.fields.image.fields.file.url + '?w=1200'" alt>
    <!-- render blog posts -->
    <ul>
      <li v-for="(post,index) in posts" :key="index">
        <nuxt-link :to="'blog/' + post.fields.slug" class="title">
          <img :src="post.fields.heroImage.fields.file.url + '?fit=scale&w=350&h=196'" alt>
          <p>{{ post.fields.slug}}</p>
          {{post.fields.path}}
        </nuxt-link>
      </li>
    </ul>
  </div>
</template>

<script>
import { createClient } from '~/plugins/contentful.js'

const client = createClient()

export default {
  // `env` is available in the context object
  asyncData({ env }) {
    return Promise.all([
      // fetch the owner of the blog
      client.getEntries({
        'sys.id': env.CTF_PERSON_ID
      }),
      // fetch all blog posts sorted by creation date
      client.getEntries({
        content_type: env.CTF_BLOG_POST_TYPE_ID,
        order: '-sys.createdAt'
      })
    ])
      .then(([entries, posts]) => {
        // return data that should be available
        // in the template
        console.log(posts.items)
        return {
          person: entries.items[0],
          posts: posts.items,
          content_type: entries
        }
      })
      .catch(console.error)
  }
}
</script>