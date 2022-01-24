<template>
  <div class="home">
    <form-exhibition></form-exhibition>
  </div>
</template>

<script>
// @ is an alias to /src
import FormExhibition from "@/components/FormExhibition.vue";

export default {
  name: "Home",
  components: {
    FormExhibition,
  },
};
</script>

<style lang="scss">
.home {
  max-width: 440px;
  margin: 0 auto;
}
</style>
