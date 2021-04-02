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
          :to="baseLink + item.to"
          router
          exact
        >
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="item.title"/>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar
      :clipped-left="clipped"
      fixed
      app
    >
      <v-app-bar-nav-icon @click.stop="drawer = !drawer"/>
      <v-btn
        icon
        @click.stop="clipped = !clipped"
      >
        <v-icon>mdi-application</v-icon>
      </v-btn>
      <v-toolbar-title v-text="title"/>
      <v-spacer/>

      <a
        v-for="locale in availableLocales"
        :key="locale.code"
        :href="(locale.code === 'es')?'/' :'/'+ locale.code">
        <v-toolbar-title v-text="locale.name"/>
      </a>

    </v-app-bar>
    <v-main>
      <v-container>
        <nuxt/>
      </v-container>
    </v-main>

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
  data() {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: 'mdi-apps',
          title: this.$t('Home'),
          to: "/"
        },
        {
          icon: 'mdi-book-open-page-variant-outline',
          title: this.$t('Papers'),
          to: '/posts/' + this.$t('papers_post')
        },
        {
          icon: 'mdi-beaker-outline',
          title: this.$t('Research'),
          to: '/posts/' + this.$t('research_post')
        },
        {
          icon: 'mdi-school',
          title: this.$t('Teaching'),
          to: '/posts/' + this.$t('teaching_post')
        },
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'Hugo J. Bello'
    }
  },
  methods: {
  },
  computed: {
    availableLocales() {
      return this.$i18n.locales.filter(i => i.code !== this.$i18n.locale)
    },
    baseLink() {
      let base = ""
      if (this.$i18n.locale === "es") {
        base = ""
      } else {
        base = "/en"
      }
      console.log(this.$i18n.locale,base)
      return base
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
    "teaching_post": "docencia.en",
    "papers_post": "papers.en",
    "research_post": "research.en"
  },
  "es": {
    "Home": "Inicio",
    "Research": "Investigación",
    "Teaching": "Docencia",
    "Papers": "Artículos",
    "About": "Acerca de",
    "teaching_post": "docencia.es",
    "papers_post": "papers.es",
    "research_post": "research.es"
  }
}
</i18n>
