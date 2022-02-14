<template>
  <div class="container">
    <h2 class="form-title">Subscribe to newsletter</h2>
    <form
      ref="form"
      id="form-summet"
      class="form-delegate-summit"
      @submit.prevent="sendForm"
    >
      <div
        v-for="field in fields"
        :key="field.id"
        class="form-delegate-summit__group"
      >
        <label :for="field.id" class="form-delegate-summit__label">
          {{ field.label }}
        </label>
        <input
          ref="inputForm"
          :type="field.type"
          :id="field.id"
          v-model="postData[field.model]"
          class="form-delegate-summit__input"
          :placeholder="field.placeholder"
        />
        <span ref="textError" class="form-delegate-summit__text-error" />
        <img
          src="@/assets/error-icon.svg"
          alt="error"
          class="form-delegate-summit__error-img"
        />
      </div>

      <div class="form-delegate-summit__checkbox">
        <input
          v-model="isChecked"
          type="checkbox"
          id="checkbox-summit"
          class="form-delegate-summit__checkbox"
        />
        <label for="checkbox-summit">
          I agree with the terms of personal data processing
        </label>
      </div>

      <button class="form-delegate-summit__button button-submit">
        sign up
      </button>
    </form>
    <modal-status
      v-show="modalShow"
      :statusModal="status"
      @sendForm="sendForm"
      @close="modalShow = !modalShow"
    >
    </modal-status>
  </div>
</template>

<script>
import axios from "axios";
import ModalStatus from "@/components/ModalStatus.vue";
const regexpEmail =
  /^([a-z0-9_-]+\.)*[a-z0-9_-]+@[a-z0-9_-]+(\.[a-z0-9_-]+)*\.[a-z]{2,6}$/;

export default {
  name: "form-subscribe-en",
  components: { ModalStatus },
  data() {
    return {
      status: 0,
      isChecked: false,
      isValid: false,
      isLoading: false,
      modalShow: false,
      fields: [
        {
          id: "user-first-name",
          label: "Name",
          type: "text",
          model: "userFirstName",
          placeholder: "Anna",
        },
        {
          id: "user-email",
          label: "E-mail",
          type: "email",
          model: "userEmail",
          placeholder: "vasileva@gmail.com",
        },
      ],
      postData: {
        valuationForm: "Subscribe to newsletter",
        userFirstName: "",
        userEmail: "",
      },
      errorMessage: "",
      textSendMessage: "",
    };
  },
  mounted() {
    this.validateForm();
  },
  methods: {
    /**
     * * Валидируем форму перед отправкой данных
     */
    validateForm() {
      /**
       * * Получаем поля и пишем проверки
       *
       * @param elem {HTML Element}
       */
      const validate = (elem) => {
        if (elem.id === "user-first-name") {
          if (elem.value === "") {
            elem
              .closest(".form-delegate-summit__group")
              .classList.remove("form-delegate-summit__group-succes");
            elem.nextElementSibling.innerText = "This field is required!";
            elem
              .closest(".form-delegate-summit__group")
              .classList.add("form-delegate-summit__group-errors");
          } else {
            elem.nextElementSibling.innerText = "";
            elem
              .closest(".form-delegate-summit__group")
              .classList.remove("form-delegate-summit__group-errors");
            elem
              .closest(".form-delegate-summit__group")
              .classList.add("form-delegate-summit__group-succes");
          }
        }

        /**
         * * Валидируем поле E-mail
         */
        if (elem.id === "user-email") {
          if (elem.value === "") {
            elem
              .closest(".form-delegate-summit__group")
              .classList.remove("form-delegate-summit__group-succes");
            elem.nextElementSibling.innerText = "This field is required!";
            elem
              .closest(".form-delegate-summit__group")
              .classList.add("form-delegate-summit__group-errors");
          } else if (!regexpEmail.test(elem.value) && elem.value !== "") {
            elem.nextElementSibling.innerText = "Please enter a valid E-mail*";
            elem
              .closest(".form-delegate-summit__group")
              .classList.add("form-delegate-summit__group-errors");
          } else {
            elem.nextElementSibling.innerText = "";
            elem
              .closest(".form-delegate-summit__group")
              .classList.remove("form-delegate-summit__group-errors");
            elem
              .closest(".form-delegate-summit__group")
              .classList.add("form-delegate-summit__group-succes");
          }
        }
      };

      this.$refs.inputForm.forEach((elem) => {
        elem.addEventListener("blur", () => {
          validate(elem);
        });
      });
    },

    /**
     * * Отправляем данные на почту и в телеграм канал
     */
    async sendForm() {
      this.$refs.inputForm.forEach((elem) => {
        if (elem.value === "") {
          this.isValid = false;
          elem
            .closest(".form-delegate-summit__group")
            .classList.remove("form-delegate-summit__group-succes");
          elem.nextElementSibling.innerText = "This field is required!";
          elem
            .closest(".form-delegate-summit__group")
            .classList.add("form-delegate-summit__group-errors");
        } else {
          this.isValid = true;
          elem.nextElementSibling.innerText = "";
          elem
            .closest(".form-delegate-summit__group")
            .classList.remove("form-delegate-summit__group-errors");
          elem
            .closest(".form-delegate-summit__group")
            .classList.add("form-delegate-summit__group-succes");
        }
      });

      if (!this.isChecked) {
        alert("Please agree to the terms of personal data processing!");
      }
      if (!this.isChecked || (!this.validateForm() && !this.isValid)) return;

      this.modalShow = true;
      this.isLoading = true;
      this.status = 1;
      this.textSendMessage = "Sending data";

      await fetch("telegram.php", {
        method: "POST",
        mode: "cors",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(this.postData),
      });

      await axios
        .post("send.php", this.postData)
        .then((responce) => {
          if (responce.status == 200) {
            this.modalShow = true;
            this.isLoading = false;
            this.status = 2;
            this.textSendMessage =
              "Application successfully sent, we will contact you shortly";
          }
        })
        .catch((error) => {
          this.modalShow = true;
          this.isLoading = false;
          this.status = 3;
          this.textSendMessage =
            "Your request has not been sent, please try again";
          console.log(error);
        });
    },
  },
};
</script>

<style lang="scss" scoped>
.form-title {
  padding-top: 10px;
  font-size: 16px;
  line-height: 24px;
  text-align: center;
}
.form-delegate-summit {
  position: relative;
  height: 100%;
  padding: 20px 0;
  color: #1a1a1a;

  &__label {
    position: absolute;
    top: -5px;
    left: 15px;
    display: inline-block;
    padding: 0 8px;
    font-size: 10px;
    line-height: 12px;
    background: #fff;
    z-index: 2;
  }

  &__input {
    position: relative;
    display: flex;
    align-items: center;
    width: 100%;
    min-height: 38px;
    padding: 0 22px;
    border: 1px solid rgba(#1c1819, 0.2);
    border-radius: 4px;

    &:focus {
      border: 1px solid rgba(0, 123, 255, 0.7);
    }

    &::placeholder {
      font-size: 12px;
      line-height: 20px;
      color: rgba(#1c1819, 0.2);
    }
  }

  &__checkbox {
    display: flex;
    align-items: center;
    justify-content: center;

    label {
      font-size: 12px;
      line-height: 16px;
      color: rgba(#1a1a1a, 0.7);
    }

    input {
      width: 20px;
      height: 20px;
      margin-right: 10px;
      border: 1px solid rgba(#1c1819, 0.2);
      border-radius: 8px;
      cursor: pointer;
    }
  }

  &__button {
    display: flex;
    align-items: center;
    justify-content: center;
    min-width: 280px;
    margin: 30px auto 0;
    padding: 12px 0;
    background: #007bff;
    font-size: 12px;
    line-height: 14px;
    text-transform: uppercase;
    color: #fff;
    outline: none;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }

  &__error-img {
    position: absolute;
    top: 36%;
    right: 16px;
    transform: translateY(-45%);
    visibility: hidden;
  }

  &__text-error {
    display: flex;
    align-items: center;
    min-height: 14px;
    font-size: 10px;
    line-height: 12px;
    color: #ff3333;
  }
}

.form-delegate-summit__group {
  position: relative;
  margin-bottom: 12px;
}

.form-delegate-summit__group-errors {
  input {
    border: 1px solid #ff3333;
  }

  .form-delegate-summit__error-img {
    visibility: visible;
  }
}

.form-delegate-summit__group-succes {
  input {
    border: 1px solid #28a745;
  }

  .form-delegate-summit__error-img {
    visibility: hidden;
  }
}
</style>
