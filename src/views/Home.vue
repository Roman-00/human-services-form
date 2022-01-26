<template>
  <div class="home">
    <popup
      v-show="popupShow"
      :popupTitle="popupTitle"
      @close="popupShow = !popupShow"
    >
      <form-summit v-if="status === 2"></form-summit>
      <form-exhibition v-else-if="status === 1"></form-exhibition>
    </popup>
  </div>
</template>

<script>
// @ is an alias to /src
import FormSummit from "@/components/FormSummit.vue";
import FormExhibition from "@/components/FormExhibition.vue";
import Popup from "@/components/Popup.vue";

export default {
  name: "Home",
  components: {
    FormSummit,
    FormExhibition,
    Popup,
  },
  data() {
    return {
      popupShow: false,
      popupTitle: "",
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
        this.popupTitle = "Заявка на участие в выставке";
        this.popupShow = true;
        this.status = 1;
      } else if (+this.$route.query.count === 2) {
        this.popupTitle = "Стать делегатом Саммита";
        this.popupShow = true;
        this.status = 2;
      }
    },

    /**
     * * Закрываем модальное окно
     */
    close() {
      this.popupShow = !this.popupShow;
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
