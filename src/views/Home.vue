<template>
  <div class="home">
    <form-summit v-if="status === 2" />
    <form-exhibition v-else-if="status === 1" />
    <form-summit-en v-if="status === 3" />
    <form-exhibition-en v-else-if="status === 4" />
    <form-subscribe v-if="status === 5" />
    <form-subscribe-en v-else-if="status === 6" />
  </div>
</template>

<script>
// @ is an alias to /src
import FormSummit from "@/components/FormSummit.vue";
import FormExhibition from "@/components/FormExhibition.vue";
import FormSummitEn from "@/components/FormSummitEn.vue";
import FormExhibitionEn from "@/components/FormExhibitionEn.vue";
import FormSubscribe from "@/components/FormSubscribe.vue";
import FormSubscribeEn from "@/components/FormSubscribeEn.vue";

export default {
  name: "Home",
  components: {
    FormSummit,
    FormExhibition,
    FormSubscribe,
    FormSubscribeEn,
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
      } else if (+this.$route.query.count === 5) {
        this.status = 5;
      } else if (+this.$route.query.count === 6) {
        this.status = 6;
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
