<template>
  <v-layout column justify-center align-center>
    <v-flex>
      <v-list two-line>
        <template v-for="(item, index) in news" class="item-row">
          <v-list-item :key="item.title">
            <img class="floating-img" :src="item.urlToImage" />
            <v-list-item-content>
              <v-list-item-title @click="viewDetail(index)">{{
                item.title
              }}</v-list-item-title>
              <v-list-item-subtitle>{{ item.content }}</v-list-item-subtitle>
            </v-list-item-content>
          </v-list-item>
        </template>
      </v-list>
    </v-flex>
  </v-layout>
</template>

<script>
import axios from 'axios'

export default {
  components: {},
  asyncData({ params, error }) {
    // i found this tric to disable CORS errors
    const proxyurl = 'https://cors-anywhere.herokuapp.com/'

    const URL =
      // eslint-disable-next-line no-template-curly-in-string
      `${proxyurl}https://newsapi.org/v2/top-headlines?country=us&apiKey=cc372bf8f6cf4ffe9e162bf5860fc4fe`
    return axios
      .get(`${URL}`, {
        headers: {
          'Content-Type': 'application/json',
          Accept: 'application/json',
          'Access-Control-Allow-Origin': '*'
        }
      })
      .then((res) => {
        return {
          news: res.data.articles
        }
      })
      .catch((e) => {
        error(e)
      })
  },
  data() {
    // initialise the news object.
    return {
      news: [
        {
          source: {
            id: null,
            name: 'Marketwatch.com'
          },
          author: 'Barbara Kollmeyer',
          title:
            'European stocks boosted by German data in holiday-thinned trading session - MarketWatch',
          description: 'Bayer climbs on news of Roundup settlements',
          url:
            'https://www.marketwatch.com/story/european-stocks-climb-in-holiday-thinned-trading-session-2020-05-25',
          urlToImage:
            'https://s.marketwatch.com/public/resources/images/MW-IH125_church_ZG_20200525043828.jpg',
          publishedAt: '2020-05-25T09:39:51Z',
          content:
            'European stocks rose to kick off the week on Monday, with investors inspired by countries reopening from pandemic shutdowns, and an upbeat business sentiment survey out of Germany. But trade was thin… [+3430 chars]'
        },
        {
          source: {
            id: 'fox-news',
            name: 'Fox News'
          },
          author: 'Fox News',
          title:
            '3-state manhunt for UConn student wanted in 2 homicides: cops - Fox News',
          description:
            'Three states are conducting a manhunt Monday for a University of Connecticut accused of two brutal slayings and believed to be armed with several stolen guns had stretched from Connecticut to New Jersey and Pennsylvania.',
          url:
            'https://www.foxnews.com/us/3-state-manhunt-for-uconn-student-wanted-in-2-homicides-cops',
          urlToImage:
            'https://static.foxnews.com/foxnews.com/content/uploads/2020/05/OS7R4O3VIJD6VJNLXCGJ7DDINQ.jpg',
          publishedAt: '2020-05-25T09:14:47Z',
          content:
            'Three states are conducting a manhunt Monday for a University of Connecticut accused of two brutal slayings and believed to be armed with several stolen guns had stretched from Connecticut to New Jer… [+1714 chars]'
        }
      ]
    }
  },
  methods: {
    viewDetail(index) {
      // store the selected item in the localstorage then navigate to detail page
      localStorage.setItem(
        'item',
        JSON.stringify({ index, article: this.news[index] })
      )
      this.$router.push('detail/' + index)
    }
  }
}
</script>
