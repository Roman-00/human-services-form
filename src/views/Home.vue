<template>
  <div class="home">
    <form-summit v-if="status === 2"></form-summit>
    <form-exhibition v-else-if="status === 1"></form-exhibition>
    <form-summit-en v-if="status === 3"></form-summit-en>
    <form-exhibition-en v-else-if="status === 4"></form-exhibition-en>
  </div>
</template>

<script>
// @ is an alias to /src
import FormSummit from "@/components/FormSummit.vue";
import FormExhibition from "@/components/FormExhibition.vue";
import FormSummitEn from "@/components/FormSummitEn.vue";
import FormExhibitionEn from "@/components/FormExhibitionEn.vue";

export default {
  name: "Home",
  components: {
    FormSummit,
    FormExhibition,
    FormSummitEn,
    FormExhibitionEn,
  },
  data() {
    return {
      status: 0,
    };
  },
  mounted() {
    this.checkWindowLocation();
  },
  methods: {
    /**
     * * Проверяем адресную строку на параметры
     */
    checkWindowLocation() {
      if (+this.$route.query.count === 1) {
        this.status = 1;
      } else if (+this.$route.query.count === 2) {
        this.status = 2;
      } else if (+this.$route.query.count === 3) {
        this.status = 3;
      } else if (+this.$route.query.count === 4) {
        this.status = 4;
      }
    },
  },
};
</script>

<style lang="scss">
.home {
  max-width: 440px;
  margin: 0 auto;
}
</style>
