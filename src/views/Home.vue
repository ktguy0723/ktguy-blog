<template>
  <div class="home">
    <v-text-field 
      class="ma-3"
      v-model="keyword"
      label="Search Title"
      outlined
      hide-details
      clearable
    ></v-text-field>
    <div v-for="(section, index) in Object.keys(articles)" :key="index">
      <v-list-item>
        <v-list-item-content>
          <v-list-item-title>{{section}}</v-list-item-title>
          <div v-for="entry in articles[section]" :key="entry.id">
            <v-list-item
              link 
              three-line
              @click="$router.push({name: entry.id})"
            >
              <v-list-item-content>
                <v-list-item-title>{{entry.title}}</v-list-item-title>
                <v-list-item-subtitle>{{entry.date}}</v-list-item-subtitle>
                <v-list-item-subtitle>{{entry.description}}</v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>
            <v-divider></v-divider>
          </div>
        </v-list-item-content>
      </v-list-item>
    </div>


  </div>
</template>

<script>
import BLOGENTRIES from '@/assets/blogs.json'
export default {
  name: 'home',
  data() {
    return {
      keyword: '',
    }
  },
  computed: {
    entries() {
      return BLOGENTRIES
    },
    articles() {
      let articles = {}
      Object.keys(this.entries).forEach(section => {
        articles[section] = []
        let keywordLower = ''
        if(this.keyword) {
          keywordLower = this.keyword.toLowerCase()
        }
        this.entries[section].forEach(article => {
          const titleLower = article.title.toLowerCase()
          if(titleLower.indexOf(keywordLower) !== -1) { 
            articles[section].push(article)
          }
        })
      })
      return articles
    }
  }
}
</script>
<style lang="scss" scoped>

</style>