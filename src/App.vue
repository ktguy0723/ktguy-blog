<template>
  <v-app id="inspire">
    <v-navigation-drawer 
      app
      v-model="drawer"
    >
      <v-list-item>
        <v-list-item-content>
          <v-list-item-title class="text-h6">
            ktguy tech blog
          </v-list-item-title>
          <v-list-item-subtitle>
            @ktguy0723
          </v-list-item-subtitle>
        </v-list-item-content>
      </v-list-item>

      <v-divider></v-divider>

      <v-list
        dense
        nav
      >
        <v-list-item
          v-for="item in items"
          :key="item.title"
          :to="item.to"
          :href="item.href"
          link
        >
          <v-list-item-icon>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-icon>

          <v-list-item-content>
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar app>
      <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>

      <v-toolbar-title>ktguy tech blog</v-toolbar-title>
    </v-app-bar>

    <v-main>
      <v-breadcrumbs
        divider=">"
        :items = "breadcrumbs"
      ></v-breadcrumbs>
      <router-view />
    </v-main>
  </v-app>
</template>

<script>
  export default {
    data: () => ({
      drawer: null,
      items: [
        { title: 'Home', icon: 'mdi-view-dashboard', to:'/' },
        { title: 'github', icon: 'mdi-image', href:"https://github.com/ktguy0723" },
      ],
    }),
    computed: {
      breadcrumbs() {
        // パスの取得 homeの場合
        const paths = this.$route.path
        if(paths === '/') {
          return [{
            text: 'home',
            to: '/'
          }]
        }
        
        let to = ''
        const items = paths.split('/').map((path, i) => {
          if(i === 0) {
            return {            
              text: 'home',
              to: '/'
            }
          }
          to += `/${path}`
          return {
            text: path === '' ? 'home': path,
            to: to
          }
        });
        return items
      }
    }
  }
</script>

