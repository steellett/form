<template>
  <form action="post" class="registration-form" @submit.prevent="submitHandler">
    <h2 class="form-title">Заполните форму</h2>
    <div class="personal-data">
      <h2 class="form-part-title">Персональные данные</h2>
      <div class="input-field">
        <label class="input-field__label">
          Фамилия
          <span v-if="!$v.surname.required">*</span>
        </label>
        <input type="text" class="input-field__data" v-model.trim="surname" />
        <span
          class="input-field__invalid"
          v-if="$v.surname.$dirty && !$v.surname.required"
        >Вы должны указать фамилию</span>
      </div>
      <div class="input-field">
        <label class="input-field__label">
          Имя
          <span v-show="!name">*</span>
        </label>
        <input type="text" class="input-field__data" v-model.trim="name" />
        <span
          class="input-field__invalid"
          v-if="$v.name.$dirty && !$v.name.required"
        >Укажите ваше имя</span>
      </div>
      <div class="input-field">
        <label class="input-field__label">Отчество</label>
        <input type="text" class="input-field__data" v-model.trim="secondname" />
      </div>
      <div class="input-field">
        <label class="input-field__label">
          Дата рождения
          <span v-show="!birthdate">*</span>
        </label>
        <input type="date" class="input-field__data" v-model.trim="birthdate" />
        <span
          class="input-field__invalid"
          v-if="$v.birthdate.$dirty && !$v.birthdate.required"
        >Укажите дату вашего рождения</span>
      </div>
      <div class="input-field">
        <label class="input-field__label">
          Номер телефона
          <span v-show="!phone">*</span>
          <span class="tip">(Должен начинаться с 7)</span>
        </label>
        <input
          type="text"
          class="input-field__data"
          v-model.trim="phone"
          @blur="{phone=correctPhone}"
        />
        <span
          class="input-field__invalid"
          v-if="!$v.phone.isLetter && $v.phone.required"
        >Телефон должен содержать только цифры</span>
        <span
          class="input-field__invalid"
          v-if="$v.phone.$dirty && !$v.phone.required"
        >Укажите ваш номер телефона</span>
      </div>
      <div class="input-field">
        <label class="input-field__label">Пол</label>
        <select type="date" class="input-field__select" v-model.trim="gender">
          <option value selected disabled>Выберите ваш пол</option>
          <option value="man">Мужской</option>
          <option value="woman">Женский</option>
        </select>
      </div>
      <div class="input-field">
        <label class="input-field__label">
          Группа клиентов
          <span v-if="!$v.clientsgroup.required">*</span>
        </label>
        <select
          type="date"
          class="input-field__select-multiple"
          multiple
          size="3"
          v-model.trim="clientsgroup"
        >
          <option value="VIP">VIP</option>
          <option value="problem">Проблемные</option>
          <option value="OMS">ОМС</option>
        </select>
        <span
          class="input-field__invalid"
          v-if="$v.clientsgroup.$dirty && !$v.clientsgroup.required"
        >Укажите вашу группу</span>
      </div>
      <div class="input-field">
        <label class="input-field__label">Лечащий врач</label>
        <select type="date" class="input-field__select" v-model.trim="doctor">
          <option value selected disabled>Выберите врача</option>
          <option value="Ivanov">Иванов</option>
          <option value="Zaharov">Захаров</option>
          <option value="Chernysheva">Чернышева</option>
        </select>
      </div>
      <div class="input-field">
        <input
          type="checkbox"
          class="input-field__data input-field__checkbox"
          v-model.trim="sendSMS"
        />
        <label class="input-field__label">Не отправлять СМС</label>
      </div>
    </div>
    <div class="address">
      <h2 class="form-part-title">Адресные данные</h2>

      <div class="input-field">
        <label class="input-field__label">Индекс</label>
        <input type="text" class="input-field__data" v-model.trim="index" />
      </div>
      <div class="input-field">
        <label class="input-field__label">Страна</label>
        <input type="text" class="input-field__data" v-model.trim="country" />
      </div>
      <div class="input-field">
        <label class="input-field__label">Область, край, республика</label>
        <input type="text" class="input-field__data" v-model.trim="oblast" />
      </div>
      <div class="input-field">
        <label class="input-field__label">
          Город
          <span v-show="!city">*</span>
        </label>
        <input type="text" class="input-field__data" v-model.trim="city" />
        <span
          class="input-field__invalid"
          v-if="$v.city.$dirty && !$v.city.required"
        >Укажите ваш город</span>
      </div>
      <div class="input-field">
        <label class="input-field__label">Улица</label>
        <input type="text" class="input-field__data" v-model.trim="street" />
      </div>
      <div class="input-field">
        <label class="input-field__label">Дом</label>
        <input type="text" class="input-field__data" v-model.trim="house" />
      </div>
    </div>
    <div class="document">
      <h2 class="form-part-title">Документ</h2>

      <div class="input-field">
        <label class="input-field__label">
          Документ
          <span v-show="!document">*</span>
        </label>
        <select type="date" class="input-field__select" v-model.trim="document">
          <option value selected disabled>Выберите документ</option>
          <option value="Ivanov">Паспорт</option>
          <option value="Zaharov">Свидетельство о рождении</option>
          <option value="Chernysheva">Вод. удостоверение</option>
        </select>
        <span
          class="input-field__invalid"
          v-if="$v.document.$dirty && !$v.document.required"
        >Выберите документ</span>
      </div>
      <div class="input-field">
        <label class="input-field__label">Серия</label>
        <input type="text" class="input-field__data" v-model.trim="documentSeria" />
      </div>
      <div class="input-field">
        <label class="input-field__label">Номер</label>
        <input type="text" class="input-field__data" v-model.trim="documentNumber" />
      </div>
      <div class="input-field">
        <label class="input-field__label">Кем выдан</label>
        <input type="text" class="input-field__data" v-model.trim="documentWhoGave" />
      </div>
      <div class="input-field">
        <label class="input-field__label">
          Дата выдачи
          <span v-show="!documentDate">*</span>
        </label>
        <input type="date" class="input-field__data" v-model.trim="documentDate" />
        <span
          class="input-field__invalid"
          v-if="$v.documentDate.$dirty && !$v.documentDate.required"
        >Выберите дату выдачи</span>
      </div>
    </div>
    <div class="register-button">
      <button class="auth-submit" type="submit">Зарегистрироваться</button>
    </div>
    <div class="tip">*-поля обязательные для заполнения</div>
    <div class="suсcess-registration" v-if="success" @click="closeSuccess">
      <img src="@/assets/happydoctor.jpg" alt />
      <p>Вы успешно зарегистрировались</p>
    </div>
  </form>
</template>

<script>
import { required, minLength, maxLength } from "vuelidate/lib/validators";

const isLetter = (value) => {
  return !/\D\+/.test(value);
};

export default {
  data() {
    return {
      surname: null,
      name: null,
      secondname: null,
      birthdate: null,
      phone: null,
      gender: null,
      clientsgroup: [],
      doctor: null,
      sendSMS: null,
      index: null,
      country: null,
      oblast: null,
      city: null,
      street: null,
      house: null,
      document: null,
      documentSeria: null,
      documentNumber: null,
      documentWhoGave: null,
      documentDate: null,
      success: false,
    };
  },
  validations: {
    surname: { required },
    name: { required },
    birthdate: { required },
    phone: {
      required,
      isLetter,
      minLength: minLength(11),
      maxLength: maxLength(11),
    },
    clientsgroup: { required },
    city: { required },
    document: { required },
    documentDate: { required },
  },
  computed: {
    correctPhone() {
      if (this.phone && this.phone.length == 11 && /^8/.test(this.phone)) {
        return this.phone.replace(8, 7);
      }
      if (this.phone && this.phone.length == 12 && /^\+7/.test(this.phone)) {
        return this.phone.replace("+7", 7);
      }
      return this.phone;
    },
  },
  methods: {
    submitHandler() {
      if (this.$v.$invalid) {
        this.$v.$touch();
        return;
      }
      const formData = {
        surname: this.surname,
        name: this.name,
        secondname: this.secondname,
        birthdate: this.birthdate,
        phone: this.correctPhone,
        gender: this.gender,
        clientsgroup: this.clientsgroup,
        doctor: this.doctor,
        sendSMS: this.sendSMS,
        index: this.index,
        country: this.country,
        oblast: this.oblast,
        city: this.city,
        street: this.street,
        house: this.house,
        document: this.document,
        documentSeria: this.documentSeria,
        documentNumber: this.documentNumber,
        documentWhoGave: this.documentWhoGave,
        documentDate: this.documentDate,
      };
      console.log(formData);
      this.success = true;
    },
    closeSuccess() {
      this.success = !this.success;
    },
  },
};
</script>

<style lang="scss">
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  background: #22c1c3;
  background: linear-gradient(38deg, #22c1c3, #fdbb2d);
}
.registration-form {
  width: 50%;
  margin: 2em auto;
  background: #fff;
  padding: 2em;
  border-radius: 20px;
  box-shadow: 0 0 30px 2px #5f5fe7;
}
.form-title {
  font-size: 2em;
  color: #e02746;
  text-shadow: 0 0 1px #0021b6;
}
.form-part-title {
  width: 100%;
  margin: 1em;
  color: #e02746;
}
.address,
.document,
.personal-data {
  width: 100%;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
}
.form-content,
.input-field {
  display: flex;
  flex-wrap: wrap;
}
.input-field {
  display: flex;
  position: relative;
  width: 45%;
  padding-bottom: 1.2em;
  align-items: center;
  flex-shrink: 0.5;
  margin-right: auto;
}
.input-field__label {
  width: 100%;
  text-align: left;
  font-size: 1em;
  color: #f1528f;
}
.input-field__data {
  width: 100%;
  height: 2em;
  padding-left: 1em;
  margin-right: auto;
  border: none;

  border: 1px solid #a09d9d;
  border-radius: 5px;
}
.input-field__checkbox {
  width: initial;
}
.input-field__select {
  height: 2em;
}
.input-field__select,
.input-field__select-multiple {
  width: 50%;
  border: 1px solid #a09d9d;
  border-radius: 5px;
}
.input-field__select-multiple {
  overflow: hidden;
}
.register-button {
  margin-top: 2em;
}
.auth-submit {
  height: 2em;
  width: 50%;
  color: #fff;
  font-size: 1.5em;
  border: none;
  border-radius: 10px;
  text-align: center;
  background: #22c1c3;
  background: linear-gradient(38deg, #22c1c3, #fd2d73 50%);
  box-shadow: 0 0 5px 1px black;
}
.suсcess-registration {
  position: fixed;
  height: 100vh;
  width: 100vw;
  top: 0;
  left: 0;
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: center;
  background: rgb(102, 204, 204);
  background: linear-gradient(
    38deg,
    rgba(102, 204, 204, 1) 69%,
    rgba(234, 253, 45, 0.9920343137254902) 89%
  );
  color: #fff;
  font-size: 3em;
  z-index: 10;
  img {
    width: 30%;
    box-shadow: 0 0 20px 5px rgb(101, 212, 212);
  }
  p {
    width: 100%;
  }
}
.input-field__invalid {
  position: absolute;
  left: 0;
  bottom: 0;
  color: #fff;
  background: rgba(255, 0, 0, 0.616);
  padding: 0.2em;
  font-size: 0.75em;
  border-radius: 5px;
}
.tip {
  color: black;
  font-size: 0.8em;
}
.error {
  border: 1px solid red;
}
@media screen and (max-width: 940px) {
  .registration-form {
    width: 80%;
  }
  .input-field {
    width: 45%;
  }
  .auth-submit {
    height: 3em;
  }
  .input-field__select,
  .input-field__select-multiple {
    width: 100%;
  }
  .suсcess-registration {
    img {
      width: 60%;
    }
  }
}
@media screen and (max-width: 640px) {
  .input-field,
  .registration-form {
    width: 90%;
  }
  .auth-submit {
    margin: 1em;
    height: 3em;
    width: 80%;
  }
  .suсcess-registration {
    img {
      width: 80%;
    }
    p {
      font-size: 30px;
    }
  }
}
@media screen and (max-width: 390px) {
  .registration-form {
    width: 100%;
  }
  .auth-submit {
    font-size: 1.2em;
  }
}
</style>
