<template>
  <BContainer>
    <h4>Личные данные</h4>
    <!-- <h2>Данные: {{ formData }}</h2> -->
    <BForm class="form" @submit.prevent="formSubmit" novalidate>
      <BRow>
        <BCol cols="3">
          <BFormGroup label="Фамилия" label-for="lastname">
            <BFormInput
              :class="{ error: validationForm.lastName }"
              id="lastname"
              type="text"
              v-model="formData.lastName"
            />
            <span class="errorMessage" v-if="validationForm.lastName"
              >Некорректные данные</span
            >
          </BFormGroup>
        </BCol>
        <BCol cols="3">
          <BFormGroup label="Имя" label-for="firsname">
            <BFormInput
              :class="{ error: validationForm.firstName }"
              id="firsname"
              type="text"
              v-model="formData.firstName"
            />
            <span class="errorMessage" v-if="validationForm.firstName"
              >Некорректные данные</span
            >
          </BFormGroup>
        </BCol>
        <BCol cols="3">
          <BFormGroup label="Отчество" label-for="patronymic">
            <BFormInput
              :class="{ error: validationForm.patronymic }"
              id="patronymic"
              type="text"
              v-model="formData.patronymic"
            />
            <span class="errorMessage" v-if="validationForm.patronymic"
              >Некорректные данные</span
            >
          </BFormGroup>
        </BCol>
      </BRow>

      <BRow>
        <BCol cols="4">
          <BFormGroup label="Дата рождения" label-for="datebirth">
            <BFormInput
              :class="{ error: validationForm.dateOfBirth }"
              id="datebirth"
              type="text"
              placeholder="дд.мм.гггг"
              v-model="formData.dateOfBirth"
            />
            <span class="errorMessage" v-if="validationForm.dateOfBirth"
              >Некорректные данные</span
            >
          </BFormGroup>
        </BCol>
      </BRow>

      <BRow>
        <BCol cols="9">
          <BFormGroup label="E-mail" label-for="email">
            <BFormInput
              :class="{ error: validationForm.email }"
              id="email"
              type="email"
              v-model="formData.email"
              placeholder="example@example.com"
            />
            <span class="errorMessage" v-if="validationForm.email"
              >Некорректные данные</span
            >
          </BFormGroup>
        </BCol>
      </BRow>

      <BRow>
        <BCol>
          <BFormGroup label="Пол">
            <BFormRadioGroup>
              <BFormRadio v-model="formData.gender" value="male"
                >Мужской</BFormRadio
              >
              <BFormRadio v-model="formData.gender" value="female"
                >Женский</BFormRadio
              >
            </BFormRadioGroup>
          </BFormGroup>
        </BCol>
      </BRow>

      <h4>Паспортные данные</h4>

      <BRow>
        <BCol cols="4" v-click-outside="hideDropdown">
          <BFormGroup label="Гражданство" label-for="citizenship">
            <BFormInput
              id="citizenship"
              v-model="searchCountry"
              @focus="isDropdownOpen = true"
            />
            <div class="country-dropdown" v-if="isDropdownOpen" cols="4">
              <ul>
                <li
                  v-for="country in filterCountries"
                  :key="country.id"
                  @click="onClick(country)"
                >
                  {{ country.nationality }}
                </li>
              </ul>
            </div>
          </BFormGroup>
        </BCol>
      </BRow>
      <template v-if="formData.citizenship === ''"></template>
      <template v-else-if="formData.citizenship === 'Russia'">
        <BRow>
          <BCol cols="3">
            <BFormGroup label="Серия паспорта" label-for="passportSeries">
              <BFormInput
                :class="{ error: validationForm.passportSeries }"
                id="passportSeries"
                type="text"
                v-model="formData.passportSeries"
              />
              <span class="errorMessage" v-if="validationForm.passportSeries"
                >Некорректные данные</span
              >
            </BFormGroup>
          </BCol>
          <BCol cols="3">
            <BFormGroup label="Номер паспорта" label-for="passportNumber">
              <BFormInput
                :class="{ error: validationForm.passportNumber }"
                id="passportNumber"
                type="text"
                v-model="formData.passportNumber"
              />
              <span class="errorMessage" v-if="validationForm.passportNumber"
                >Некорректные данные</span
              >
            </BFormGroup>
          </BCol>
          <BCol cols="3">
            <BFormGroup label="Дата выдачи" label-for="passportIssueDate">
              <BFormInput
                id="passportIssueDate"
                type="text"
                placeholder="дд.мм.гггг"
                v-model="formData.passportIssueDate"
              />
            </BFormGroup>
          </BCol>
        </BRow>
      </template>

      <template v-else>
        <BRow>
          <BCol cols="5">
            <BFormGroup label="Фамилия на латинице" label-for="lastname_en">
              <BFormInput
                :class="{ error: validationForm.lastName_en }"
                id="lastname_en"
                type="text"
                v-model="formData.lastName_en"
              />
              <span class="errorMessage" v-if="validationForm.lastName_en"
                >Некорректные данные</span
              >
            </BFormGroup>
          </BCol>

          <BCol cols="5">
            <BFormGroup label="Имя на латинице" label-for="firsname_en">
              <BFormInput
                :class="{ error: validationForm.firstName_en }"
                id="firsname_en"
                type="text"
                v-model="formData.firstName_en"
              />
              <span class="errorMessage" v-if="validationForm.firstName_en"
                >Некорректные данные</span
              >
            </BFormGroup>
          </BCol>
        </BRow>

        <BRow>
          <BCol cols="3">
            <BFormGroup
              label="Номер паспорта"
              label-for="foreignPassportNumber"
            >
              <BFormInput
                :class="{ error: validationForm.foreignPassportNumber }"
                id="foreignPassportNumber"
                type="text"
                v-model="formData.foreignPassportNumber"
              />
              <span
                class="errorMessage"
                v-if="validationForm.foreignPassportNumber"
                >Некорректные данные</span
              >
            </BFormGroup>
          </BCol>

          <BCol cols="3">
            <BFormGroup label="Страна выдачи" label-for="countryOfIssue">
              <BFormSelect
                id="countryOfIssue"
                :options="issueCountry"
                v-model="formData.countryOfIssue"
              ></BFormSelect>
            </BFormGroup>
          </BCol>

          <BCol cols="3">
            <BFormGroup label="Тип паспорта" label-for="typeOfPassport">
              <BFormSelect
                id="typeOfPassport"
                :options="passportTypes"
                v-model="formData.typeOfPassport"
              ></BFormSelect>
            </BFormGroup>
          </BCol>
        </BRow>
      </template>

      <BRow>
        <BCol>
          <BFormGroup label="Менял ли фамилию или имя?">
            <BFormRadioGroup>
              <BFormRadio v-model="formData.changeLastName" value="no"
                >Нет</BFormRadio
              >
              <BFormRadio v-model="formData.changeLastName" value="yes"
                >Да</BFormRadio
              >
            </BFormRadioGroup>
          </BFormGroup>
        </BCol>
      </BRow>

      <template v-if="formData.changeLastName === 'yes'">
        <BRow>
          <BCol cols="5">
            <BFormGroup
              label="Предыдущая фамилия"
              label-for="beforeChangeLastName"
            >
              <BFormInput
                :class="{ error: validationForm.beforeChangeLastName }"
                id="beforeChangeLastName"
                type="text"
                v-model="formData.beforeChangeLastName"
              />
              <span
                :class="errorMessage"
                v-if="validationForm.beforeChangeLastName"
                >Некорректные данные</span
              >
            </BFormGroup>
          </BCol>

          <BCol cols="5">
            <BFormGroup
              label="Предыдущее имя"
              label-for="beforeChangeFirstName"
            >
              <BFormInput
                :class="{ error: validationForm.beforeChangeFirstName }"
                id="beforeChangeFirstName"
                type="text"
                v-model="formData.beforeChangeFirstName"
              />
              <span
                class="errorMessage"
                v-if="validationForm.beforeChangeFirstName"
                >Некорректные данные</span
              >
            </BFormGroup>
          </BCol>
        </BRow>
      </template>

      <div class="button-container d-flex">
        <BButton variant="primary" size="lg" type="submit">Сохранить</BButton>
      </div>
    </BForm>
  </BContainer>
</template>

<script>
import citizenships from "@/assets/data/citizenships.json";
import passportTypes from "@/assets/data/passport-types.json";
import ClickOutside from "vue-click-outside";
// import { debounce } from "vue-debounce";

export default {
  name: "Form",
  directives: {
    ClickOutside,
  },

  data: () => ({
    formData: {
      lastName: "",
      firstName: "",
      patronymic: "",
      dateOfBirth: "",
      email: "",
      gender: "",
      citizenship: "",
      passportSeries: "",
      passportNumber: "",
      passportIssueDate: "",
      foreignPassportNumber: "",
      countryOfIssue: "",
      typeOfPassport: "",
      changeLastName: "",
      lastName_en: "",
      firstName_en: "",
      beforeChangeLastName: "",
      beforeChangeFirstName: "",
    },
    allCountries: citizenships,
    passportTypes: [],
    issueCountry: [],
    isDropdownOpen: false,
    searchCountry: "",

    regExpDic: {
      lastName: /[А-Яа-я]+/,
      firstName: /[А-Яа-я]+/,
      patronymic: /[А-Яа-я]+/,
      beforeChangeLastName: /[А-Яа-я]+/,
      beforeChangeFirstName: /[А-Яа-я]+/,
      email:
        /^([a-zA-Z0-9_\-.]+)@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.)|(([a-zA-Z0-9-]+\.)+))([a-zA-Z]{1,5}|[0-9]{1,3})(\]?)$/,
      passportSeries: /^[0-9]{4}$/,
      passportNumber: /^[0-9]{6}$/,
      foreignPassportNumber: /[0-9]+/,
      lastName_en: /^[a-zA-Z]+/,
      firstName_en: /^[a-zA-Z]+/,
    },

    validationForm: {
      lastName: false,
      firstName: false,
      patronymic: false,
      dateOfBirth: false,
      beforeChangeLastName: false,
      beforeChangeFirstName: false,
      email: false,
      passportSeries: false,
      passportNumber: false,
      foreignPassportNumber: false,
      lastName_en: false,
      firstName_en: false,
    },
  }),

  created() {
    this.issueCountry = this.getCitizenshipOptions();
    this.passportTypes = this.getPassportTypes();
  },

  computed: {
    filterCountries() {
      if (this.searchCountry === "") {
        return this.allCountries;
      }
      return this.allCountries.filter((country) => {
        if (
          country.nationality
            .toLowerCase()
            .startsWith(this.searchCountry.toLowerCase())
        ) {
          return country;
        }
      });
    },
  },

  methods: {
    getCitizenshipOptions() {
      let options = citizenships.map((item) => {
        return {
          value: item.nationality,
          text: item.nationality,
        };
      });
      return options;
    },

    getPassportTypes() {
      let options = passportTypes.map((item) => {
        return {
          value: item.type,
          text: item.type,
        };
      });
      return options;
    },

    hideDropdown() {
      this.isDropdownOpen = false;
    },

    onClick(selectedCountry) {
      this.formData.citizenship = selectedCountry.nationality;
      this.formData.passportNumber = "";
      this.formData.passportSeries = "";
      this.formData.passportIssueDate = "";
      this.formData.foreignPassportNumber = "";
      this.formData.countryOfIssue = "";
      this.formData.typeOfPassport = "";
      this.formData.lastName_en = "";
      this.formData.firstName_en = "";
      this.hideDropdown();
    },

    formSubmit() {
      this.validation();
      this.validationDate();
      if (!this.checkValidationForm()) {
        console.log(JSON.stringify(this.formData));
      } else {
        console.error("ИСПРАВЬТЕ ДАННЫЕ");
      }
    },

    validation() {
      for (let key in this.formData) {
        if (!this.regExpDic[key] || this.formData[key] === "") {
          this.validationForm[key] = false;
        } else if (!this.regExpDic[key].test(this.formData[key])) {
          this.validationForm[key] = true;
        } else {
          this.validationForm[key] = false;
        }
      }
    },

    checkValidationForm() {
      console.log(this.validationForm);
      return Object.values(this.validationForm).find((item) => item === true);
    },

    validationDate() {
      let formatDate = new Date(
        this.formData.dateOfBirth.split(".").reverse().join(".")
      );
      let currentDate = new Date();

      if (formatDate.getTime() < currentDate.getTime()) {
        this.validationForm.dateOfBirth = false;
      } else {
        this.validationForm.dateOfBirth = true;
      }
    },
  },
};
</script>

<style scoped>
.button-container {
  justify-content: flex-end;
}
.country-dropdown {
  cursor: pointer;
  border: 1px solid black;
  overflow-y: scroll;
  height: 300px;
}

li {
  list-style-type: none;
}

ul {
  margin-left: 0;
  padding-left: 0;
}

.error {
  border: 1px solid red;
}

.errorMessage {
  color: red;
}
</style>
