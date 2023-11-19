<template>
  <div id="app">
    <TextAlert :text-message="'Новый клиент успешно создан'" :is-visible="isAlertVisible"/>
    <form ref="clientCreateForm" @submit.prevent="submitForm">
      <h1>Создание Клиента</h1>

      <div class="form-group">
        <label for="lastName">Фамилия*</label>
        <input type="text" id="lastName"
               v-model="formData.lastName"
               :class="{invalid: $v.formData.lastName.$dirty && !$v.formData.lastName.required}"
               @blur="$v.formData.lastName.$touch"
        >
        <div class="error-message" v-if="$v.formData.lastName.$dirty && !$v.formData.lastName.required">
          Это поле обязательно для заполнения.
        </div>
      </div>

      <div class="form-group">
        <label for="firstName">Имя*</label>
        <input type="text" id="firstName"
               v-model="formData.firstName"
               :class="{invalid: $v.formData.firstName.$dirty && !$v.formData.firstName.required}"
               @blur="$v.formData.firstName.$touch"
        >
        <div class="error-message" v-if="$v.formData.firstName.$dirty && !$v.formData.firstName.required">
          Это поле обязательно для заполнения.
        </div>
      </div>

      <div class="form-group">
        <label for="patronymic">Отчество</label>
        <input type="text" id="patronymic" v-model="formData.patronymic">
      </div>

      <div class="form-group">
        <label for="birthdate">Дата рождения*</label>
        <input type="date" id="birthdate"
               v-model="formData.birthdate"
               :class="{invalid: $v.formData.birthdate.$dirty && !$v.formData.birthdate.required}"
               @blur="$v.formData.birthdate.$touch"
        >
        <div class="error-message" v-if="$v.formData.birthdate.$dirty && !$v.formData.birthdate.required">
          Это поле обязательно для заполнения.
        </div>
      </div>

      <div class="form-group">
        <label for="phoneNumber">Номер телефона*</label>
        <input type="tel" id="phoneNumber"
               v-model="formData.phoneNumber"
               :class="{invalid: $v.formData.phoneNumber.$dirty && !$v.formData.phoneNumber.required}"
               @blur="$v.formData.phoneNumber.$touch"
        >
        <div class="error-message" v-if="$v.formData.phoneNumber.$dirty && !$v.formData.phoneNumber.required">
          Это поле обязательно для заполнения.
        </div>
        <div class="error-message" v-if="$v.formData.phoneNumber.$dirty && !$v.formData.phoneNumber.phoneValidator">
          Номер должен состоять из 11 цифр и начинаться с цифры 7.
        </div>
      </div>

      <div class="form-group">
        <label>Пол</label>
        <div class="radio-group">
          <label for="male">Мужской</label>
          <input type="radio" id="male" v-model="formData.gender" value="male">

          <label for="female">Женский</label>
          <input type="radio" id="female" v-model="formData.gender" value="female">
        </div>
      </div>

      <div class="form-group">
        <label for="clientGroup">Группа клиентов*</label>
        <select id="clientGroup" multiple
                v-model="formData.clientGroup"
                :class="{invalid: $v.formData.clientGroup.$dirty && !$v.formData.clientGroup.required}"
                @blur="$v.formData.clientGroup.$touch"
        >
          <option value="VIP">VIP</option>
          <option value="Problematic">Проблемные</option>
          <option value="OMS">ОМС</option>
        </select>
        <div class="error-message" v-if="$v.formData.clientGroup.$dirty && !$v.formData.clientGroup.required">
          Это поле обязательно для заполнения.
        </div>
      </div>

      <div class="form-group">
        <label for="attendingDoctor">Лечащий врач</label>
        <select id="attendingDoctor" v-model="formData.attendingDoctor">
          <option value="Ivanov">Иванов</option>
          <option value="Zakharov">Захаров</option>
          <option value="Chernysheva">Чернышева</option>
        </select>
      </div>

      <div class="form-check">
        <input type="checkbox" id="noSms" v-model="formData.noSms">
        <label for="noSms">Не отправлять СМС</label>
      </div>

      <div class="form-group-border">
        <h2>Адрес</h2>

        <div class="form-group">
          <label for="index">Индекс</label>
          <input type="text" id="index" v-model="formData.address.index">
        </div>

        <div class="form-group">
          <label for="country">Страна</label>
          <input type="text" id="country" v-model="formData.address.country">
        </div>

        <div class="form-group">
          <label for="region">Область</label>
          <input type="text" id="region" v-model="formData.address.region">
        </div>

        <div class="form-group">
          <label for="city">Город*</label>
          <input type="text" id="city"
                 v-model="formData.address.city"
                 :class="{invalid: $v.formData.address.city.$dirty && !$v.formData.address.city.required}"
                 @blur="$v.formData.address.city.$touch"
          >
          <div class="error-message" v-if="$v.formData.address.city.$dirty && !$v.formData.address.city.required">
            Это поле обязательно для заполнения.
          </div>
        </div>

        <div class="form-group">
          <label for="street">Улица</label>
          <input type="text" id="street" v-model="formData.address.street">
        </div>

        <div class="form-group">
          <label for="house">Дом</label>
          <input type="text" id="house" v-model="formData.address.house">
        </div>
      </div>

      <div class="form-group-border">
        <h2>Паспорт</h2>

        <div class="form-group">
          <label for="documentType">Тип документа*</label>
          <select id="documentType"
                  v-model="formData.passport.documentType"
                  :class="{invalid: $v.formData.passport.documentType.$dirty && !$v.formData.passport.documentType.required}"
                  @blur="$v.formData.passport.documentType.$touch"
          >
            <option value="Passport">Паспорт</option>
            <option value="BirthCertificate">Свидетельство о рождении</option>
            <option value="DriverLicense">Вод. удостоверение</option>
          </select>
          <div class="error-message"
               v-if="$v.formData.passport.documentType.$dirty && !$v.formData.passport.documentType.required">
            Это поле обязательно для заполнения.
          </div>
        </div>

        <div class="form-group">
          <label for="passportSeries">Серия</label>
          <input type="text" id="passportSeries" v-model="formData.passport.series">
        </div>

        <div class="form-group">
          <label for="passportNumber">Номер</label>
          <input type="text" id="passportNumber" v-model="formData.passport.number">
        </div>

        <div class="form-group">
          <label for="issuedBy">Кем выдан</label>
          <input type="text" id="issuedBy" v-model="formData.passport.issuedBy">
        </div>

        <div class="form-group">
          <label for="issueDate">Дата выдачи*</label>
          <input type="date" id="issueDate"
                 v-model="formData.passport.issueDate"
                 :class="{invalid: $v.formData.passport.issueDate.$dirty && !$v.formData.passport.issueDate.required}"
                 @blur="$v.formData.passport.issueDate.$touch"
          >
          <div class="error-message"
               v-if="$v.formData.passport.issueDate.$dirty && !$v.formData.passport.issueDate.required">
            Это поле обязательно для заполнения.
          </div>
        </div>
      </div>

      <button type="submit"
              :class="{invalid: $v.$invalid}"
      >Отправить</button>
    </form>
  </div>
</template>

<script>
import {required} from "vuelidate/lib/validators";
import TextAlert from "@/components/TextAlert.vue";
const phoneValidator = (value) => /^7\d{10}$/.test(value)

export default {
  name: 'App',
  components: {TextAlert},
  data() {
    return {
      formData: {
        lastName: '',
        firstName: '',
        patronymic: '',
        birthdate: '',
        phoneNumber: '',
        gender: '',
        clientGroup: [],
        attendingDoctor: '',
        noSms: false,
        address: {
          index: '',
          country: '',
          region: '',
          city: '',
          street: '',
          house: '',
        },
        passport: {
          documentType: '',
          series: '',
          number: '',
          issuedBy: '',
          issueDate: '',
        },
      },
      isAlertVisible: false
    };
  },
  validations: {
    formData: {
      lastName: {required, $autoDirty: true,},
      firstName: {required},
      birthdate: {required},
      phoneNumber: {required, phoneValidator},
      clientGroup: {required},
      address: {
        city: {required}
      },
      passport: {
        documentType: {required},
        issueDate: {required},
      },
    }
  },
  methods: {
    submitForm() {
      this.$v.$touch();
      if (!this.$v.$invalid) {
        this.isAlertVisible = true;
        setTimeout(() => {
          this.isAlertVisible = false;
        }, 2000);
      }
    },
  },
}
</script>

<style lang="scss">
body {
  font-family: 'Roboto', sans-serif;
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 0;
}

#app {
  max-width: 600px;
  width: 100%;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  background-color: #fff;
  padding: 20px;
  border-radius: 8px;

  form {
    display: flex;
    flex-direction: column;

    .form-group {
      margin-bottom: 20px;

      label {
        font-size: 16px;
        font-weight: bold;
      }

      .radio-group {
        display: flex;
        border: 1px solid #ddd;
        padding: 15px;
        border-radius: 8px;
        margin-top: 5px;

        input[type="radio"] {
          margin: 0 auto;
          transform: scale(2);
          width: auto;
        }
      }

      input,
      select {
        padding: 12px;
        border: 1px solid #ddd;
        border-radius: 4px;
        font-size: 16px;
        width: 100%;
        box-sizing: border-box;
        margin-top: 5px;
      }

      .invalid {
        border: 1px solid red;
      }
    }

    .form-check {
      display: flex;
      align-items: center;
      margin-bottom: 20px;

      input {
        margin-right: 8px;
      }

      label {
        font-size: 16px;
      }
    }

    .form-group-border {
      border: 1px solid #ddd;
      padding: 15px;
      border-radius: 8px;
      margin-bottom: 20px;

      label {
        font-size: 18px;
        font-weight: bold;
      }

      input,
      select {
        padding: 12px;
        border: 1px solid #ddd;
        border-radius: 4px;
        font-size: 16px;
        width: 100%;
        box-sizing: border-box;
      }
    }

    .error-message {
      color: #dc3545;
      margin-top: 10px;
    }

    button {
      background-color: #007bff;
      color: #fff;
      padding: 12px;
      border: none;
      border-radius: 4px;
      cursor: pointer;
      font-size: 16px;
      transition: background-color 0.3s;

      &:hover {
        background-color: #0056b3;
      }

      &.invalid {
        opacity: 0.5;
        cursor: not-allowed;
      }
    }
  }
}
</style>
