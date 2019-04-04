<template>
  <div>
    <header class="blog header">
      <div class="foreground">
        <div class="page-bar wrapper">
          <a href="/" class="person-name">John Doe</a>
          <Navigation></Navigation>
        </div>
        <div class="page-info wrapper">
          <h2>Blog</h2>
        </div>
      </div>
    </header>

    <section class="body-container">
      <div class="items-bar wrapper">
        <h2>All articles ({{ posts.length }})</h2>
      </div>
      <ul>
        <li v-for="(post,index) in posts" :key="index">
          <nuxt-link :to="'blog/' + post.fields.slug" class="title">
            <img :src="post.fields.heroImage.fields.file.url + '?fit=scale&w=350&h=196'" alt>
            <p>{{ post.fields.slug}}</p>
            {{post.fields.path}}
          </nuxt-link>
        </li>
      </ul>
    </section>
  </div>
</template>

<script>
import { createClient } from '~/plugins/contentful.js'

const client = createClient()

export default {
  asyncData({ env, params }) {
    return client
      .getEntries({
        content_type: env.CTF_BLOG_POST_TYPE_ID,
        order: '-sys.createdAt'
      })
      .then(entries => {
        return {
          posts: entries.items
        }
      })
  }
}
</script>
