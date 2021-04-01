<template>
  <div>
    <h1
      class="my-8 max-w-full m-auto text-3xl text-center font-medium"
    >
      {{ post.title }}
    </h1>
    <h3 class="py-4 text-center uppercase">{{ post.description }}</h3>
    <nuxt-content :document="post" class="leading-loose" />

    <br></br>

    <p>{{$t('Article last updated:')}} {{ formatDate(post.date) }}</p>
    <p> <span v-if="post.tags" v-for="tag in post.tags" :key="tag">
      <nuxt-link :to="'/tag?tag='+tag"><v-chip>{{tag}}</v-chip></nuxt-link>
    </span></p>

    <br></br>
    <v-card
      v-if="post.toc && post.toc.length >0"
      elevation="0"
    >
      <v-card-title>{{$t('Table of contents')}}</v-card-title>
      <v-card-text>
        <ul>
          <li v-for="link of post.toc" :key="link.id" :class="{ 'py-2': link.depth === 2, 'ml-2 pb-2': link.depth === 3 }">
            <NuxtLink :to="`#${link.id}`">{{ link.text }}</NuxtLink>
          </li>
        </ul>
      </v-card-text>
    </v-card>

  </div>
</template>

<script>
export default {
  async asyncData({ $content, params, error, app }) {
    const locale = app.i18n.locale

    const post = await $content(`posts/${params.slug}`)
      .fetch()
      .catch(err => {
        error({ statusCode: 404, message: "Página no encontrada" });
      });

    return {
      post
    };
  },
  methods: {
    formatDate(date) {
      const options = { year: 'numeric', month: 'long', day: 'numeric' }
      return new Date(date).toLocaleDateString('en', options)
    }
  }

};
</script>

<style>
.icon.icon-link {
  background-image: url('~assets/svg/icon-hashtag.svg');
  display: inline-block;
  width: 20px;
  height: 20px;
  color: white !important;
  background-size: 20px 20px;
}
</style>

<i18n>
{
  "en": {
    "Article last updated:": "Article last updated:",
    "Table of contents": "Table of contents"
  },
  "es": {
    "Article last updated:": "Articulo actualizado ",
    "Table of contents": "Contenidos"

  }
}
</i18n>
