<template>
  <div>
    <h1 class="title">{{$t('Posts in tag')}} {{$route.query["category"]}}</h1>
    <p class="subtitle-1">{{ index.description }}</p>
    <nuxt-content :document="index" class="leading-loose"/>
    <div class="container">
      <div v-for="(post, index) in posts" :key="index">
        <nuxt-link :to="getLink(post.path)" class="underline">
          <v-card class="card">
            <v-img
              v-if="post.image"
              height="150"
              :src="post.image"
            ></v-img>
            <v-card-title>
              {{post.title}}
            </v-card-title>
            <v-card-text>
              {{post.description}}
            </v-card-text>
            <v-card-subtitle>
              {{formatDate(post.date)}}
            </v-card-subtitle>

          </v-card>
        </nuxt-link>
        <br></br>
      </div>
    </div>

    <div class="text-center">
      <v-pagination
        v-model="page"
        :length="6"
        @input="nextPage"
      ></v-pagination>
    </div>
  </div>


</template>


<script>

const fetchPosts = async ($content, error, category,  locale, page, itemsPerPage) => {
  const skip = (page-1)*itemsPerPage
  console.log(page, skip)
  return await $content("posts")
    .only(["title", "path", "date", "description", "image", "tags"])
    .limit(itemsPerPage)
    .skip(skip)
    .sortBy('date')
    .where({
      language: locale,
      categories:{ $contains: category }
    })
    .fetch()
    .catch(err => {
      error({statusCode: 404, message: "Página no encontrada"});
    });
}

export default {
  async asyncData({$content, route, params, error, app}) {
    const category = route.query["category"] || ""

    const page = 1
    const itemsPerPage = 10

    const locale = app.i18n.locale

    const contentFile = (locale === "es") ? "index.es" : "index.en"


    const index = await $content(contentFile)
      .fetch()
      .catch(err => {
        error({statusCode: 404, message: "index not found"});
      });

    const posts = await fetchPosts($content, error, category, locale, page, itemsPerPage)

    return {
      index,
      posts,
      page,
      locale,
      itemsPerPage,
      category: category
    };
  },
  methods: {
    async nextPage(){
      this.posts = await fetchPosts(this.$content, this.error, this.tag, this.locale, this.page, this.itemsPerPage )
      console.log(this.posts)
    },
    formatDate(date) {
      const options = { year: 'numeric', month: 'long', day: 'numeric' }
      return new Date(date).toLocaleDateString('en', options)
    },
    getLink(path){
      if (this.locale === "es"){
        return path
      } else {
        return `${this.locale}${path}`
      }
    }
  }

};
</script>

<style>
a:link {
  text-decoration: none;
}
.card{
  margin: 0 auto;
  max-width: 600px;
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
