<template>
  <div class="popup">
    <div class="popup-overlay" @click="$emit('close')">
      <div class="popup-content" @click.stop>
        <button class="popup-close" @click="$emit('close')">
          <img src="@/assets/close.svg" alt="Закрываем модалку" />
        </button>
        <h3>{{ popupTitle }}</h3>
        <div class="popup-body">
          <slot></slot>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "popup",
  props: {
    popupTitle: {
      type: String,
    },
  },
  mounted() {
    document.body.addEventListener("keyup", (e) => {
      if (e.keyCode === 27) {
        this.$emit("close");
      }
    });
  },
};
</script>

<style lang="scss" scoped>
.popup {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  width: 100%;
  height: 100%;
  z-index: 10;
}

.popup-overlay {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
  background: rgba(#1c1819, 0.6);
}

.popup-content {
  position: relative;
  padding: 20px;
  width: 100%;
  max-width: 440px;
  height: auto;
  background: #fff;
  z-index: 11;

  h3 {
    margin-left: 8px;
    font-size: 18px;
    font-weight: 600;
    line-height: 28px;
    text-align: center;
    color: #1c1819;

    @media screen and (max-width: 380px) {
      font-size: 16px;
      line-height: 24px;
    }
  }
}

.popup-close {
  position: absolute;
  top: 0;
  right: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px;
  background: transparent;
  border: none;
  outline: none;
  cursor: pointer;
}
</style>
