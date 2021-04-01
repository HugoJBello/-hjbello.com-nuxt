<template>
  <div>
    <h1
      class="my-8 max-w-full m-auto text-3xl text-center font-medium"
    >
      {{ post.title }}
    </h1>
    <h3 class="py-4 text-center uppercase">{{ post.description }}</h3>
    <nuxt-content :document="post" class="leading-loose" />

    <p>Article last updated: {{ formatDate(post.date) }}</p>
    <p> <span v-if="post.tags" v-for="tag in post.tags" :key="tag"><v-chip>{{tag}}</v-chip></span></p>
  </div>
</template>

<script>
export default {
  async asyncData({ $content, params, error }) {
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
  color: white;
  background-size: 20px 20px;
}
</style>
