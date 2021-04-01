<template>
  <v-app dark>
    <v-navigation-drawer
      v-model="drawer"
      :mini-variant="miniVariant"
      :clipped="clipped"
      fixed
      app
    >
      <v-list>
        <v-list-item
          v-for="(item, i) in items"
          :key="i"
          :to="item.to"
          router
          exact
        >
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="item.title" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar
      :clipped-left="clipped"
      fixed
      app
    >
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-btn
        icon
        @click.stop="clipped = !clipped"
      >
        <v-icon>mdi-application</v-icon>
      </v-btn>
      <v-toolbar-title v-text="title" />
      <v-spacer />
      <v-btn
        icon
        @click.stop="rightDrawer = !rightDrawer"
      >
        <v-icon>mdi-menu</v-icon>
      </v-btn>
    </v-app-bar>
    <v-main>
      <v-container>
        <nuxt />
      </v-container>
    </v-main>
    <v-navigation-drawer
      v-model="rightDrawer"
      :right="right"
      temporary
      fixed
    >
      <v-list>
        <v-list-item @click.native="right = !right">
          <v-list-item-action>
            <v-icon light>
              mdi-repeat
            </v-icon>
          </v-list-item-action>
          <v-list-item-title>Switch drawer (click me)</v-list-item-title>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-footer
      :absolute="!fixed"
      app
    >
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data () {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: 'mdi-apps',
          title: this.$t('Home'),
          to: '/'
        },
        {
          icon: 'mdi-apps',
          title: this.$t('Papers'),
          to: '/posts/' + this.$t('papers_post')
        },
        {
          icon: 'mdi-apps',
          title:  this.$t('Research'),
          to: '/category?category=research'
        },
        {
          icon: 'mdi-apps',
          title:  this.$t('Teaching'),
          to: '/category?category=teaching'
        },
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'Hugo J. Bello'
    }
  }
}
</script>

<i18n>
{
  "en": {
    "Research": "Research",
    "Teaching": "Teaching",
    "Papers": "Papers",
    "About": "About",
    "teaching_post": "teaching.en",
    "papers_post": "papers.en",
    "research_post": "research.en"
  },
  "es": {
    "Home": "Inicio",
    "Research": "Investigación",
    "Teaching": "Docencia",
    "Papers": "Artículos",
    "About": "Acerca de",
    "teaching_post": "teaching.es",
    "papers_post": "papers.es",
    "research_post": "research.es"
  }
}
</i18n>
