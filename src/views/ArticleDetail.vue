<template>
  <v-card flat>
    <v-img height="400" :src="article.image.url"></v-img>

    <v-card-title>{{ article.title }}</v-card-title>

    <v-card-text style="text-align: left">
      <div v-html="sanitizedBody"></div>
    </v-card-text>
  </v-card>
</template>

<style>
 img {
    max-width: 100%;
    height: auto;
    display: block;
}
</style>

<script>
import axios from "axios";
import sanitizeHtml from "sanitize-html";

export default {
  name: "ArticleDetail",

  data: () => ({
    article: {}
  }),

  computed: {
    sanitizedBody() {
      // imgタグのみ使えるようにする
      return sanitizeHtml(this.article.body, {
        allowedTags: sanitizeHtml.defaults.allowedTags.concat(["img"])
      });
    }
  },

  async mounted() {
    // idを指定して特定の記事を取得する
    const response = await axios.get(
      "https://kiku-kash.microcms.io/api/v1/articles/" +
        this.$route.params.id,
      {
        headers: { "X-MICROCMS-API-KEY": 'ca28af1304964604a52a994cbc6e05ce45e8' }
      }
    );
    this.article = response.data;
  }
};
</script>
