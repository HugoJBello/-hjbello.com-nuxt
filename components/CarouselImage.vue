<template>
  <div>
    <v-carousel class="image-carousel" cycle>
      <v-carousel-item
        eager
        v-for="item in images"
        :key="item.src"
        :src="item.src"
        reverse-transition="fade-transition"
        transition="fade-transition"
      >
        <v-row           
        align="center"
        justify="center">
          <div v-if="item.title" class="text-title-carousel">
            <a :src="item.link">{{ item.title }}</a>
          </div>
        </v-row>
      </v-carousel-item>
    </v-carousel>
  </div>
</template>
<script>
export default {
  data() {
    return {
      images: [
        {
          src: require("../pages/img/average_month_crime_news.png"),
          title: "current on-process paper",
          link:"",
        },
        {
          src: require("../pages/img/argeline_citruline_correlation.png"),
          title: "l-Arginine and Beetroot Extract Supplementation in the Prevention of Sarcopenia ",
          link:"https://www.mdpi.com/1424-8247/15/3/290",
        },
        {
          src: require("../pages/img/ansiedad_escala_golberg_vs_resiliencia_boxplot.png"),
          title: "current on-process paper",
          link:"",
        },
        { src: require("../pages/img/citrulline_boxplots.png"),
          title: "L-Citrulline Supplementation and Exercise in the Management of Sarcopenia",
          link:"https://doi.org/10.3390/nu13093133",},
        { src: require("../pages/img/discriminant_edited_diagram.png"),
         title: "Effect of Glutamine Supplementation on Muscular Damage Biomarkers in Professional Basketball Players",
          link:"https://doi.org/10.3390/nu13062073"},
        { src: require("../pages/img/grpah_diagnose_pallete_clean.png"),
         title: "Diagnostic stability in Bipolar Disorder: A follow-up study in 130.000 patient-years",
          link:"https://doi.org/10.4088/jcp.20m13764"},
        { src: require("../pages/img/suicidios_trend.png"),
        title:"News Media and Suicide: Using Big Data Techniques to Assess the Long-Term Impact",
        link:"https://doi.org/10.1080/00332747.2021.1989934" },
        { src: require("../pages/img/tags_gender_violence_big.png"),
        title: "current on-process paper",
          link:"", },
        { src: require("../pages/img/tda_example.png") },
      ],
    };
  },
  async fetch() {
    this.$forceUpdate();
  },
};
</script>


<style>
.image-carousel {
  margin: 0 auto;
  max-height: 500px;
}
.text-title-carousel {
    margin-top: 40px;
  text-align: center;
}
</style>