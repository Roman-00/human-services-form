<template>
  <div id="modal" class="modal">
    <div class="modal-wrapper" @click="$emit('close')">
      <div class="modal-content" @click.stop="">
        <button class="modal-close" @click="$emit('close')">
          <img src="@/assets/close.svg" alt="Modal Close" />
        </button>
        <div v-if="statusModal === 1" class="modal-content-succses">
          <div class="modal-content-header">
            <div class="lds-default">
              <div></div>
              <div></div>
              <div></div>
              <div></div>
              <div></div>
              <div></div>
              <div></div>
              <div></div>
              <div></div>
              <div></div>
            </div>
            <h3>Отправляем заявку!</h3>
          </div>
          <div class="modal-content-body">
            <span>Идет отправка данных... Пожалуйста подождите</span>
          </div>
        </div>

        <div v-if="statusModal === 2" class="modal-content-succses">
          <div class="modal-content-header">
            <img src="@/assets/sucses.svg" alt="Status Sucses" />
            <h3>Заявка отправлена!</h3>
          </div>
          <div class="modal-content-body">
            <span>Ожидайте, мы свяжемся с вами в&nbsp;ближайшее время</span>
            <button @click="$emit('close')">Отлично</button>
          </div>
        </div>

        <div v-if="statusModal === 3" class="modal-content-succses">
          <div class="modal-content-header">
            <img src="@/assets/error-icon.svg" alt="Status Error" />
            <h3>Заявка не отправлена!</h3>
          </div>
          <div class="modal-content-body">
            <span
              >Ваша Заявка не отправлена :( Пожалуйста попробуйте снова.</span
            >
            <button @click="$emit('sendForm')">Отправить снова</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "modal-status",
  props: {
    statusModal: {
      type: Number,
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

<style lang="scss">
.modal {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  width: 100%;
  height: 100%;
  z-index: 2;
}

.modal-wrapper {
  display: flex;
  align-items: flex-end;
  justify-content: center;
  width: 100%;
  height: 100%;
  background: rgba(#1c1819, 0.6);
}

.modal-content {
  position: relative;
  flex: 1 1 auto;
  min-height: 180px;
  padding: 16px;
  background: #fff;
  border-radius: 16px 16px 0 0;
  box-shadow: 0px 2px 10px rgba(28, 24, 25, 0.1);
}

.modal-content-header {
  display: flex;
  align-items: center;
  justify-content: center;

  h3 {
    margin-left: 8px;
    font-size: 18px;
    font-weight: 600;
    line-height: 28px;
    color: #1c1819;
  }
}

.modal-content-body {
  span {
    display: block;
    margin: 16px 0 24px;
    font-size: 16px;
    line-height: 24px;
    text-align: center;
  }

  button {
    width: 100%;
    min-height: 44px;
    font-size: 16px;
    line-height: 24px;
    border: none;
    background: #2a81dd;
    box-shadow: 0px 2px 4px rgba(28, 24, 25, 0.1);
    border-radius: 8px;
    color: #fff;
    cursor: pointer;
  }
}

.modal-close {
  position: absolute;
  top: 10px;
  right: 10px;
  padding: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 24px;
  height: 24px;
  border: none;
  outline: none;
  background: transparent;
  cursor: pointer;
}
.lds-default {
  display: inline-block;
  position: relative;
  width: 40px;
  height: 40px;
}
.lds-default div {
  position: absolute;
  width: 6px;
  height: 6px;
  background: #007bff;
  border-radius: 50%;
  animation: lds-default 1.2s linear infinite;
}
.lds-default div:nth-child(1) {
  animation-delay: 0s;
  top: 15px;
  left: 33px;
}
.lds-default div:nth-child(2) {
  animation-delay: -0.1s;
  top: 7px;
  left: 26px;
}
.lds-default div:nth-child(3) {
  animation-delay: -0.2s;
  top: 5px;
  left: 17px;
}
.lds-default div:nth-child(4) {
  animation-delay: -0.3s;
  top: 7px;
  left: 7px;
}
.lds-default div:nth-child(5) {
  animation-delay: -0.4s;
  top: 15px;
  left: 2px;
}
.lds-default div:nth-child(6) {
  animation-delay: -0.5s;
  top: 27px;
  left: 0px;
}
.lds-default div:nth-child(7) {
  animation-delay: -0.6s;
  top: 37px;
  left: 7px;
}
.lds-default div:nth-child(8) {
  animation-delay: -0.7s;
  top: 39px;
  left: 20px;
}
.lds-default div:nth-child(9) {
  animation-delay: -0.8s;
  top: 34px;
  left: 30px;
}
.lds-default div:nth-child(10) {
  animation-delay: -0.9s;
  top: 25px;
  left: 35px;
}
@keyframes lds-default {
  0%,
  20%,
  80%,
  100% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.5);
  }
}
</style>
