<template>
  <div>
    <div class="container_image">
      <img src="@/assets/logo_white.png" class="image_base"></img>
    </div>

    <v-card class="card-big" v-if="!$vuetify.breakpoint.mobile">
    <v-row class="row-container d-flex flex-no-wrap justify-space-between">
      <v-col v-for="img in images" :key="img" >
        <v-img height="120px" width="120px" :src='require("./img/"+img)'></v-img>
      </v-col>
    </v-row>
    </v-card>
    <v-card class="card-big" v-else>
      <v-row class="row-container d-flex flex-no-wrap justify-space-between">
        <v-col v-for="img in images.slice(0,6)" :key="img" >
          <v-img height="60px" width="60px" :src='require("./img/"+img)'></v-img>
        </v-col>
      </v-row>
    </v-card>


    <h1 class="text-3xl py-6">{{ index.title }}</h1>

    <p class="text-xl py-3">{{ index.description }}</p>
    <nuxt-content :document="index" class="leading-loose center"/>
    <div class="container">
      <div v-for="(post, index) in posts" :key="index">
        <nuxt-link :to="getLink(post.path)" class="underline">
          <v-card class="card">
            <div class="d-flex flex-no-wrap justify-space-between">
              <div>
                <v-card-title>
                  {{ post.title }}
                </v-card-title>
                <v-card-text>
                  {{ post.description }}
                </v-card-text>
                <v-card-subtitle>
                  {{ formatDate(post.date) }}
                </v-card-subtitle>
              </div>
              <v-avatar
                v-if="post.image"
                class="ma-3"
                size="125"
                tile
              >
                <v-img v-if="post.image"
                       :src="post.image"></v-img>
              </v-avatar>
            </div>
          </v-card>
        </nuxt-link>
        <br></br>
      </div>
    </div>

    <div class="text-center">
      <v-pagination
        v-model="page"
        :length="4"
        @input="nextPage"
      ></v-pagination>
    </div>
  </div>


</template>


<script>

const fetchPosts = async ($content, error, locale, page, itemsPerPage) => {
  const skip = (page - 1) * itemsPerPage

  const where = {language: locale}

  return await $content("posts")
    .only(["title", "path", "date", "description", "image"])
    .limit(itemsPerPage)
    .skip(skip)
    .sortBy('date', "desc")
    .where(where)
    .fetch()
    .catch(err => {
      error({statusCode: 404, message: "Página no encontrada"});
    });
}

export default {
  async asyncData({$content, route, params, error, app}) {

    const page = 1
    const itemsPerPage = 10
    const images = [
      "2020-tda.png",
      "ansiedad_escala_golberg_vs_resiliencia_boxplot.png",
      "average_month_crime_news.png",
      "citrulline_boxplots.png",
      "discriminant_edited_diagram.png",
      "graph_months_subjects.png",
      "grpah_diagnose_pallete_clean.png",
      "opinion_cis_and_trend_final.png",
      "suicidios_trend.png",
      "tags_gender_violence_big.png",
      //"tda_example.png",
      //"tda_figure.png"
    ]

    const locale = app.i18n.locale

    const contentFile = (locale === "es") ? "index.es" : "index.en"


    const index = await $content(contentFile)
      .fetch()
      .catch(err => {
        error({statusCode: 404, message: "index not found"});
      });

    const posts = await fetchPosts($content, error, locale, page, itemsPerPage)

    return {
      index,
      posts,
      page,
      locale,
      images,
      itemsPerPage,
    };
  },
  methods: {
    async nextPage() {
      this.posts = await fetchPosts(this.$content, this.error, this.locale, this.page, this.itemsPerPage)
    },
    formatDate(date) {
      const options = {year: 'numeric', month: 'long', day: 'numeric'}
      return new Date(date).toLocaleDateString('en', options)
    },
    getLink(path) {
      if (this.locale === "es") {
        return path
      } else {
        return `/${this.locale}${path}`
      }
    }
  },
  data() {
    return {
      test: "data",
      test2: "data2",
      test4: "data4"
    };
  },

};
</script>

<style>
a:link {
  text-decoration: none;
}

.card {
  margin: 0 auto;
  max-width: 600px !important;
}

.card-big {
  margin: 0 auto;
  max-width: 800px !important;
}

.row-container {
  margin-left: 20px;
  margin-right: 10px;
}

.center {
  text-align: center;
}
</style>


<i18n>
{
  "en": {
    "Posts in tag": "Posts in tag "
  },
  "es": {
    "Posts in tag": "Posts en "
  }
}
</i18n>


<style>
.container_image {
  display: flex;
  justify-content: center;
}

.image_base {
  text-align: center;
  margin: 0 auto;
  max-width: 200px;
}
</style>
