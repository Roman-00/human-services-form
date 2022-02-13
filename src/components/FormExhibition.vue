<template>
  <div class="container">
    <h2 class="form-title">Заявка на участие в выставке</h2>
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

      <div ref="formGroupSelect" class="form-delegate-summit__group-select">
        <label for="form-selected">Сфера</label>
        <select id="form-selected" v-model="postData.selected">
          <option disabled value="">Выберите один из вариантов</option>
          <option>Государственные органы</option>
          <option>Дипломатические и консульские представительства</option>
          <option>Неправительственные организации</option>
          <option>Профессиональные союзы и ассоциации</option>
          <option>Институты развития</option>
          <option>Пищевая промышленность</option>
          <option>Транспорт и логистика</option>
          <option>IT и телекоммуникации</option>
          <option>Топливная промышленность</option>
          <option>Энергетика</option>
          <option>Машиностроение</option>
          <option>
            Здравоохранение, физическая культура и социальное обеспечение
          </option>
          <option>Образование</option>
          <option>Культура и искусство</option>
          <option>Наука и научное обслуживание</option>
          <option>Финансы, кредит, страхование, пенсионное обеспечение</option>
          <option>Строительство и архитектура</option>
          <option>Консалтинг</option>
          <option>СМИ</option>
        </select>
        <span ref="textErrorSelect" class="form-delegate-summit__text-error" />
      </div>

      <div
        ref="formGroupSelectPartisipation"
        class="form-delegate-summit__group-select"
      >
        <label for="form-selected">Форма участия</label>
        <select id="form-selected" v-model="postData.partisipation">
          <option disabled value="">Выберите один из вариантов</option>
          <option>Спонсор</option>
          <option>Делегат</option>
          <option>Со стендом</option>
        </select>
        <span
          ref="textErrorSelectPartisipation"
          class="form-delegate-summit__text-error"
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
          Я согласен с условиями обработки персональных данных
        </label>
      </div>

      <button class="form-delegate-summit__button button-submit">
        Зарегистрироваться
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
  name: "form-exhibition",
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
          label: "Ваше Имя",
          type: "text",
          model: "userFirstName",
          placeholder: "Анна",
        },
        {
          id: "user-last-name",
          label: "Ваша Фамилия",
          type: "text",
          model: "userLastName",
          placeholder: "Васильева",
        },
        {
          id: "user-position",
          label: "Ваша Должность",
          type: "text",
          model: "userPosition",
          placeholder: "Корреспондент",
        },
        {
          id: "user-email",
          label: "Ваш E-mail",
          type: "email",
          model: "userEmail",
          placeholder: "vasileva@gmail.com",
        },
        {
          id: "user-phone",
          label: "Ваш Телефон",
          type: "phone",
          model: "userPhone",
          placeholder: "77073455656",
        },
        {
          id: "user-name-company",
          label: "Название Компании",
          type: "text",
          model: "userNameCompany",
          placeholder: "ИП Васильева",
        },
        {
          id: "user-country",
          label: "Страна",
          type: "text",
          model: "userCountry",
          placeholder: "Казахстан",
        },
        {
          id: "user-city",
          label: "Город",
          type: "text",
          model: "userCity",
          placeholder: "Нур-Султан",
        },
        {
          id: "user-legal-address",
          label: "Юридический адрес",
          type: "text",
          model: "userLegalAddress",
          placeholder: "Нур-Султан, ул. Гагарина, дом 32",
        },
        {
          id: "user-bin",
          label: "БИН",
          type: "text",
          model: "userBin",
          placeholder: "012354780901",
        },
      ],
      postData: {
        valuationForm: "Заявка на участие в выставке",
        userFirstName: "",
        userLastName: "",
        userPosition: "",
        userEmail: "",
        userPhone: "",
        userNameCompany: "",
        userCountry: "",
        userCity: "",
        userLegalAddress: "",
        userBin: "",
        selected: "",
        partisipation: "",
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
        if (
          elem.id === "user-first-name" ||
          elem.id === "user-last-name" ||
          elem.id === "user-position" ||
          elem.id === "user-name-company" ||
          elem.id === "user-country" ||
          elem.id === "user-city" ||
          elem.id === "user-legal-address" ||
          elem.id === "user-bin"
        ) {
          if (elem.value === "") {
            elem
              .closest(".form-delegate-summit__group")
              .classList.remove("form-delegate-summit__group-succes");
            elem.nextElementSibling.innerText =
              "Данное поле обязательно для заполнения!";
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
            elem.nextElementSibling.innerText =
              "Данное поле обязательно для заполнения!";
            elem
              .closest(".form-delegate-summit__group")
              .classList.add("form-delegate-summit__group-errors");
          } else if (!regexpEmail.test(elem.value) && elem.value !== "") {
            elem.nextElementSibling.innerText = "Введите корректный E-mail*";
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
         * * Валидируем поле с Телефоном
         */
        if (elem.id === "user-phone") {
          if (elem.value === "") {
            elem
              .closest(".form-delegate-summit__group")
              .classList.remove("form-delegate-summit__group-succes");
            elem.nextElementSibling.innerText =
              "Данное поле обязательно для заполнения!";
            elem
              .closest(".form-delegate-summit__group")
              .classList.add("form-delegate-summit__group-errors");
          } else if (elem.value.length <= 3) {
            elem.nextElementSibling.innerText = `Поле телефон состоит больше чем ${
              elem.value.length
            } ${elem.value.length > 1 ? "цифры" : "цифра"}!`;
            elem
              .closest(".form-delegate-summit__group")
              .classList.add("form-delegate-summit__group-errors");
          } else if (elem.value.length > 3) {
            elem.nextElementSibling.innerText =
              "Подсказка введите номер телефона как показано тут '77072560606'";
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

          if (elem.value.length > 11) {
            elem
              .closest(".form-delegate-summit__group")
              .classList.remove("form-delegate-summit__group-succes");
            elem.nextElementSibling.innerText =
              "В поле номер телефона состоит из 11 цифр";
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
          elem.nextElementSibling.innerText =
            "Данное поле обязательно для заполнения!";
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

      if (this.postData.selected === "") {
        this.isValid = false;
        this.$refs.formGroupSelect.classList.remove(
          "form-group-select-sucsess"
        );
        this.$refs.formGroupSelect.classList.add("form-group-select-errors");
        this.$refs.textErrorSelectPartisipation.innerText =
          "Вы не выбрали Форму участия";
      } else {
        this.isValid = true;
        this.$refs.formGroupSelect.classList.remove("form-group-select-errors");
        this.$refs.formGroupSelect.classList.add("form-group-select-sucsess");
        this.$refs.textErrorSelectPartisipation.innerText = "";
      }

      if (this.postData.partisipation === "") {
        this.isValid = false;
        this.$refs.formGroupSelectPartisipation.classList.remove(
          "form-group-select-sucsess"
        );
        this.$refs.formGroupSelectPartisipation.classList.add(
          "form-group-select-errors"
        );
        this.$refs.textErrorSelect.innerText =
          "Вы не выбрали сферу деятельности";
      } else {
        this.isValid = true;
        this.$refs.formGroupSelectPartisipation.classList.remove(
          "form-group-select-errors"
        );
        this.$refs.formGroupSelectPartisipation.classList.add(
          "form-group-select-sucsess"
        );
        this.$refs.textErrorSelect.innerText = "";
      }

      if (!this.isChecked) {
        alert(
          "Пожалуйста согласитесь с уловиями обработки персональных данных!"
        );
      }
      if (!this.isChecked || (!this.validateForm() && !this.isValid)) return;

      this.modalShow = true;
      this.isLoading = true;
      this.status = 1;
      this.textSendMessage = "Идет отправка данных";

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
              "Заявка успешно отправлена, мы свяжемся с вами в ближайшее время";
          }
        })
        .catch((error) => {
          this.modalShow = true;
          this.isLoading = false;
          this.status = 3;
          this.textSendMessage =
            "Ваша заявка не отправлена, Пожалуйста попробуйте снова";
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

  &__group-select {
    position: relative;
    margin-bottom: 10px;

    select {
      position: relative;
      width: 100%;
      min-height: 38px;
      padding: 0 22px;
      border: 1px solid rgba(#1c1819, 0.2);
      border-radius: 4px;
    }

    label {
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

.form-group-select-errors {
  select {
    border: 1px solid #ff3333;
  }
}

.form-group-select-sucsess {
  select {
    border: 1px solid #28a745;
  }
}
</style>
