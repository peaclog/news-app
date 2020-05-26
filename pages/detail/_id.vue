<template>
  <v-layout column justify-center align-center>
    <v-flex>
      <template v-if="article.title">
        <v-card hover>
          <v-img :src="article.urlToImage" class="white--text align-end">
            <v-card-title>{{ article.title }}</v-card-title>
          </v-img>
          <v-card-subtitle class="pb-0">
            {{ new Date(article.publishedAt).toLocaleString() }}
          </v-card-subtitle>
          <v-card-title>
            <h5>{{ article.description }}</h5>
          </v-card-title>
          <v-card-text>{{ article.content }}</v-card-text>
          <v-card-actions>
            <v-btn>
              <a :href="article.url" target="_blank" class="no-underline">
                Visit website
                <v-icon small>mdi-open-in-new</v-icon>
              </a>
            </v-btn>

            <v-spacer></v-spacer>

            <v-btn icon @click="$router.go(-1)">
              <v-icon>mdi-chevron-left</v-icon>
            </v-btn>
          </v-card-actions>
        </v-card>
      </template>
    </v-flex>
  </v-layout>
</template>
<script>
export default {
  asyncData({ params }) {
    const item = JSON.parse(localStorage.getItem('item')) || {}
    // simple check to prevent the empty view
    if (item && item.index === +params.id) {
      return {
        article: item.article
      }
    } else {
      // redirect to the home page

      // eslint-disable-next-line no-undef
      $nuxt.$router.push('/')
    }
  },
  data() {
    // initialize the object
    return {
      article: {}
    }
  },
  head() {
    // changing the title
    return {
      title: this.article.title
    }
  }
}
</script>
