<template>
  <div>
    <div class="uk-section">
      <div class="uk-container uk-container-large">
        <!-- <h1>{{ homepage.hero.title }}</h1> -->

        <!-- <div class="uk-column-1-2 uk-column-divide">
          <h3>{{ homepage.hero.subtitle }}</h3>
        </div> -->
        
        <Articles :articles="articles" />
      </div>
    </div>
  </div>
</template>

<script>
import Articles from "../components/Articles";
import { getMetaTags } from "../utils/seo";
import { getStrapiMedia } from "../utils/medias";

export default {
  components: {
    Articles,
  },
  async asyncData({ $strapi }) {
    return {
      articles: await $strapi.find("articles", {  status: 'published' }),
      homepage: await $strapi.find("homepage"),
      global: await $strapi.find("global"),
    };
  },
  head() {
    const { seo } = this.homepage;
    const { defaultSeo, favicon, siteName } = this.global;

    // Merge default and article-specific SEO data
    const fullSeo = {
      ...defaultSeo,
      ...seo,
    };

    return {
      titleTemplate: `%s | ${siteName}`,
      title: fullSeo.metaTitle,
      meta: getMetaTags(fullSeo),
      link: [
        {
          rel: "favicon",
          href: getStrapiMedia(favicon.url),
        },
      ],
    };
  },
};
</script>
