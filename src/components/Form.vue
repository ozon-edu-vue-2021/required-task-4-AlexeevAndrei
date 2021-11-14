<template>
  <BContainer>
    <h4>Личные данные</h4>
    <!-- <h2>Данные: {{ formData }}</h2> -->
    <BForm class="form" @submit.prevent="formSubmit" novalidate>
      <BRow>
        <BCol cols="3">
          <BFormGroup label="Фамилия" label-for="lastname">
            <BFormInput
              :class="{ error: formData.lastName.isError }"
              id="lastname"
              type="text"
              v-model="formData.lastName.value"
            />
            <span class="errorMessage" v-if="formData.lastName.isError"
              >Некорректные данные</span
            >
          </BFormGroup>
        </BCol>
        <BCol cols="3">
          <BFormGroup label="Имя" label-for="firsname">
            <BFormInput
              :class="{ error: formData.firstName.isError }"
              id="firsname"
              type="text"
              v-model="formData.firstName.value"
            />
            <span class="errorMessage" v-if="formData.firstName.isError"
              >Некорректные данные</span
            >
          </BFormGroup>
        </BCol>
        <BCol cols="3">
          <BFormGroup label="Отчество" label-for="patronymic">
            <BFormInput
              :class="{ error: formData.patronymic.isError }"
              id="patronymic"
              type="text"
              v-model="formData.patronymic.value"
            />
            <span class="errorMessage" v-if="formData.patronymic.isError"
              >Некорректные данные</span
            >
          </BFormGroup>
        </BCol>
      </BRow>

      <BRow>
        <BCol cols="4">
          <BFormGroup label="Дата рождения" label-for="datebirth">
            <BFormInput
              :class="{ error: formData.dateOfBirth.isError }"
              id="datebirth"
              type="text"
              placeholder="дд.мм.гггг"
              v-model="formData.dateOfBirth.value"
            />
            <span class="errorMessage" v-if="formData.dateOfBirth.isError"
              >Некорректные данные</span
            >
          </BFormGroup>
        </BCol>
      </BRow>

      <BRow>
        <BCol cols="9">
          <BFormGroup label="E-mail" label-for="email">
            <BFormInput
              :class="{ error: formData.email.isError }"
              id="email"
              type="email"
              v-model="formData.email.value"
              placeholder="example@example.com"
            />
            <span class="errorMessage" v-if="formData.email.isError"
              >Некорректные данные</span
            >
          </BFormGroup>
        </BCol>
      </BRow>

      <BRow>
        <BCol>
          <BFormGroup label="Пол">
            <BFormRadioGroup>
              <BFormRadio v-model="formData.gender.value" value="Мужской"
                >Мужской</BFormRadio
              >
              <BFormRadio v-model="formData.gender.value" value="Женский"
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
            <span class="errorMessage" v-if="formData.citizenship.isError"
              >Укажите гражданство</span
            >
            <div class="country-dropdown" v-if="isDropdownOpen" cols="4">
              <ul>
                <li
                  v-for="country in allCountries"
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
      <template v-if="formData.citizenship.value === ''"></template>
      <template v-else-if="formData.citizenship.value === 'Russia'">
        <BRow>
          <BCol cols="3">
            <BFormGroup label="Серия паспорта" label-for="passportSeries">
              <BFormInput
                :class="{ error: formData.passportSeries.isError }"
                id="passportSeries"
                type="text"
                v-model="formData.passportSeries.value"
              />
              <span class="errorMessage" v-if="formData.passportSeries.isError"
                >Некорректные данные</span
              >
            </BFormGroup>
          </BCol>
          <BCol cols="3">
            <BFormGroup label="Номер паспорта" label-for="passportNumber">
              <BFormInput
                :class="{ error: formData.passportNumber.isError }"
                id="passportNumber"
                type="text"
                v-model="formData.passportNumber.value"
              />
              <span class="errorMessage" v-if="formData.passportNumber.isError"
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
                v-model="formData.passportIssueDate.value"
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
                :class="{ error: formData.lastName_en.isError }"
                id="lastname_en"
                type="text"
                v-model="formData.lastName_en.value"
              />
              <span class="errorMessage" v-if="formData.lastName_en.isError"
                >Некорректные данные</span
              >
            </BFormGroup>
          </BCol>

          <BCol cols="5">
            <BFormGroup label="Имя на латинице" label-for="firsname_en">
              <BFormInput
                :class="{ error: formData.firstName_en.isError }"
                id="firsname_en"
                type="text"
                v-model="formData.firstName_en.value"
              />
              <span class="errorMessage" v-if="formData.firstName_en.isError"
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
                :class="{ error: formData.foreignPassportNumber.isError }"
                id="foreignPassportNumber"
                type="text"
                v-model="formData.foreignPassportNumber.value"
              />
              <span
                class="errorMessage"
                v-if="formData.foreignPassportNumber.isError"
                >Некорректные данные</span
              >
            </BFormGroup>
          </BCol>

          <BCol cols="3">
            <BFormGroup label="Страна выдачи" label-for="countryOfIssue">
              <BFormSelect
                id="countryOfIssue"
                :options="issueCountry"
                v-model="formData.countryOfIssue.value"
              ></BFormSelect>
            </BFormGroup>
          </BCol>

          <BCol cols="3">
            <BFormGroup label="Тип паспорта" label-for="typeOfPassport">
              <BFormSelect
                id="typeOfPassport"
                :options="passportTypes"
                v-model="formData.typeOfPassport.value"
              ></BFormSelect>
            </BFormGroup>
          </BCol>
        </BRow>
      </template>

      <BRow>
        <BCol>
          <BFormGroup label="Менял ли фамилию или имя?">
            <BFormRadioGroup>
              <BFormRadio v-model="formData.changeLastName.value" value="no"
                >Нет</BFormRadio
              >
              <BFormRadio v-model="formData.changeLastName.value" value="yes"
                >Да</BFormRadio
              >
            </BFormRadioGroup>
          </BFormGroup>
        </BCol>
      </BRow>

      <template v-if="formData.changeLastName.value === 'yes'">
        <BRow>
          <BCol cols="5">
            <BFormGroup
              label="Предыдущая фамилия"
              label-for="beforeChangeLastName"
            >
              <BFormInput
                :class="{ error: formData.beforeChangeLastName.isError }"
                id="beforeChangeLastName"
                type="text"
                v-model="formData.beforeChangeLastName.value"
              />
              <span
                class="errorMessage"
                v-if="formData.beforeChangeLastName.isError"
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
                :class="{ error: formData.beforeChangeFirstName.isError }"
                id="beforeChangeFirstName"
                type="text"
                v-model="formData.beforeChangeFirstName.value"
              />
              <span
                class="errorMessage"
                v-if="formData.beforeChangeFirstName.isError"
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
import { debounce } from "@/helpers/debounce.js";

const REGEXP_CIRILIC = /[А-Яа-я]+/;
const REGEXP_LATIN = /^[a-zA-Z]+/;
const REGEXP_EMAIL =
  /^([a-zA-Z0-9_\-.]+)@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.)|(([a-zA-Z0-9-]+\.)+))([a-zA-Z]{1,5}|[0-9]{1,3})(\]?)$/;
const REGEXP_PASSPORT_NUMBER = /^[0-9]{6}$/;
const REGEXP_PASSPORT_SERIES = /^[0-9]{4}$/;
const REGEXP_FOREIGN_PASSPORT_NUMBER = /[0-9]+/;

export default {
  name: "Form",
  directives: {
    ClickOutside,
  },

  data: () => ({
    formData: {
      lastName: {
        value: "",
        reg: REGEXP_CIRILIC,
        isError: false,
        validate: true,
      },
      firstName: {
        value: "",
        reg: REGEXP_CIRILIC,
        isError: false,
        validate: true,
      },
      patronymic: {
        value: "",
        reg: REGEXP_CIRILIC,
        isError: false,
        validate: true,
      },
      dateOfBirth: { value: "", isError: false, validate: false },
      email: { value: "", reg: REGEXP_EMAIL, isError: false, validate: true },
      gender: { value: "", isError: false, validate: false },
      citizenship: { value: "", isError: false, validate: true },
      passportSeries: {
        value: "",
        reg: REGEXP_PASSPORT_SERIES,
        isError: false,
        validate: true,
      },
      passportNumber: {
        value: "",
        reg: REGEXP_PASSPORT_NUMBER,
        isError: false,
        validate: true,
      },
      passportIssueDate: { value: "" },
      foreignPassportNumber: {
        value: "",
        reg: REGEXP_FOREIGN_PASSPORT_NUMBER,
        isError: false,
        validate: true,
      },
      countryOfIssue: { value: "", validate: false },
      typeOfPassport: { value: "", validate: false },
      changeLastName: { value: "", validate: false },
      lastName_en: {
        value: "",
        reg: REGEXP_LATIN,
        isError: false,
        validate: true,
      },
      firstName_en: {
        value: "",
        reg: REGEXP_LATIN,
        isError: false,
        validate: true,
      },
      beforeChangeLastName: {
        value: "",
        reg: REGEXP_CIRILIC,
        isError: false,
        validate: true,
      },
      beforeChangeFirstName: {
        value: "",
        reg: REGEXP_CIRILIC,
        isError: false,
        validate: true,
      },
    },

    allCountries: citizenships,
    passportTypes: [],
    issueCountry: [],
    isDropdownOpen: false,
    searchCountry: "",
    debouncedSearchCountries: null,
  }),

  created() {
    this.issueCountry = this.getCitizenshipOptions();
    this.passportTypes = this.getPassportTypes();
    this.debouncedSearchCountries = debounce(this.filterCountries, 500);
  },

  computed: {},

  methods: {
    getCitizenshipOptions() {
      return citizenships.map((item) => {
        return {
          value: item.nationality,
          text: item.nationality,
        };
      });
    },

    getPassportTypes() {
      return passportTypes.map((item) => {
        return {
          value: item.type,
          text: item.type,
        };
      });
    },

    hideDropdown() {
      this.isDropdownOpen = false;
    },

    onClick(selectedCountry) {
      this.formData.citizenship.value = selectedCountry.nationality;
      this.formData.passportNumber.value = "";
      this.formData.passportSeries.value = "";
      this.formData.passportIssueDate.value = "";
      this.formData.foreignPassportNumber.value = "";
      this.formData.countryOfIssue.value = "";
      this.formData.typeOfPassport.value = "";
      this.formData.lastName_en.value = "";
      this.formData.firstName_en.value = "";
      this.hideDropdown();
    },

    formSubmit() {
      this.validation();
      if (this.checkValidation()) {
        let res = {};
        for (let key in this.formData) {
          res[key] = this.formData[key].value;
        }
        console.log(JSON.stringify(res));
      } else {
        console.error("НЕКОРРЕКТНЫЕ ДАННЫЕ");
      }
    },

    validation() {
      for (let key in this.formData) {
        if (this.formData[key].value === "" && this.formData[key].validate) {
          this.formData[key].isError = true;
        } else if (
          this.formData[key].reg &&
          !this.formData[key].reg.test(this.formData[key].value)
        ) {
          this.formData[key].isError = true;
        } else {
          this.formData[key].isError = false;
        }
      }
      if (
        this.formData.changeLastName.value === "no" ||
        this.formData.changeLastName.value === ""
      ) {
        this.formData.beforeChangeLastName.isError = false;
        this.formData.beforeChangeFirstName.isError = false;
      }
      if (this.formData.citizenship.value === "Russia") {
        this.formData.foreignPassportNumber.isError = false;
        this.formData.lastName_en.isError = false;
        this.formData.firstName_en.isError = false;
      }
      if (this.formData.citizenship.value !== "Russia") {
        this.formData.passportSeries.isError = false;
        this.formData.passportNumber.isError = false;
      }
      this.validationDate();
    },

    checkValidation() {
      if (Object.values(this.formData).find((data) => data.isError)) {
        return false;
      }
      return true;
    },

    validationDate() {
      let formatDate = new Date(
        this.formData.dateOfBirth.value.split(".").reverse().join(".")
      );
      let currentDate = new Date();

      if (formatDate.getTime() < currentDate.getTime()) {
        this.formData.dateOfBirth.isError = false;
      } else {
        this.formData.dateOfBirth.isError = true;
      }
    },

    filterCountries(searchCountry) {
      this.allCountries = citizenships.filter((country) =>
        country.nationality
          .toLowerCase()
          .startsWith(searchCountry.toLowerCase())
      );
    },
  },

  watch: {
    searchCountry(newValue) {
      this.debouncedSearchCountries(newValue);
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
