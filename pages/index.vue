<template>
  <div>
    <h1 class="text-3xl py-6">{{ index.title }}</h1>
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
  async asyncData({ $content, params, error,app }) {

    const locale = app.i18n.locale

    const contentFile = (locale==="es")? "index.es": "index.en"


    const index = await $content(contentFile)
      .fetch()
      .catch(err => {
        error({ statusCode: 404, message: "index not found" });
      });

    const posts = await $content("posts")
      .only(["title", "path", "date"])
      .limit(5)
      .sortBy('date')
      .where({
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
