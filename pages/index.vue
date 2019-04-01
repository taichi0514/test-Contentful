<template>
  <div>
    <!-- render data of the person -->
    <h1>{{ person.fields.name }}</h1>
    <!-- render blog posts -->
    <ul>
      <li v-for="(post,index) in posts" :key="index">{{ post.fields.title }}</li>
    </ul>
    <ul>
      <li v-for="(post,index) in posts" :key="index">
        <img :src="post.fields.heroImage.fields.file.url + '?fit=scale&w=350&h=196'" alt>
      </li>
    </ul>
    <img :src="person.fields.image.fields.file.url + '?w=1200'" alt>

    <p>{{assets}}</p>
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
        return {
          person: entries.items[0],
          posts: posts.items
        }
      })
      .catch(console.error)
    client
      .getAssets()
      .then(assets => {
        return assets.items.map(function(asset) {
          var imageURL = 'https:' + asset.fields.file.url
        })
        console.log(asset.fields.file.url)
      })
      .catch(function(e) {
        console.log(e)
      })
  }
}
</script>