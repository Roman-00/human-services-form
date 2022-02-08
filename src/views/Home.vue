<template>
  <div class="home">
    <form-summit v-if="status === 2"></form-summit>
    <form-exhibition v-else-if="status === 1"></form-exhibition>
  </div>
</template>

<script>
// @ is an alias to /src
import FormSummit from "@/components/FormSummit.vue";
import FormExhibition from "@/components/FormExhibition.vue";

export default {
  name: "Home",
  components: {
    FormSummit,
    FormExhibition,
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
