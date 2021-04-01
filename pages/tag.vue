<template>
  <div>
    <h1 class="text-3xl py-6">{{$t('Posts in category')}} {{$route.query["category"]}}</h1>
    <p class="text-xl py-3">{{ index.description }}</p>
    <nuxt-content :document="index" class="leading-loose" />
    <ul class="list-disc list-inside mb-4">
      <li v-for="(post, index) in posts" :key="index">
        <nuxt-link :to="post.path" class="underline">{{ post.title }}</nuxt-link>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  async asyncData({ $content, route, params, error, app }) {
    const tag = route.query["tag"] || ""
    const locale = app.i18n.locale

    const index = await $content("category")
      .fetch()
      .catch(err => {
        error({ statusCode: 404, message: "index not found" });
      });

    const posts = await $content("posts")
      .only(["title", "path", "date" ])
      .limit(10)
      .sortBy('date')
      .where({
        tag:{ $contains: tag },
        language:locale
      })
      .fetch()
      .catch(err => {
        error({ statusCode: 404, message: "Página no encontrada" });
      });

    return {
      index,
      posts
    };
  }
};
</script>

<i18n>
{
  "en": {
    "Posts in category": "Posts in category "
  },
  "es": {
    "Posts in category": "Posts en "
  }
}
</i18n>
