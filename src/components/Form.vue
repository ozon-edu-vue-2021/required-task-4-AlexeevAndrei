<template>
  <BContainer>
    <h4>Личные данные</h4>
    <!-- <h2>Данные: {{ formData }}</h2> -->
    <BForm class="form" @submit.prevent="formSubmit">
      <BRow>
        <BCol cols="3">
          <BFormGroup label="Фамилия" label-for="lastname">
            <BFormInput id="lastname" type="text" v-model="formData.lastName" />
          </BFormGroup>
        </BCol>
        <BCol cols="3">
          <BFormGroup label="Имя" label-for="firsname">
            <BFormInput
              id="firsname"
              type="text"
              v-model="formData.firstName"
            />
          </BFormGroup>
        </BCol>
        <BCol cols="3">
          <BFormGroup label="Отчество" label-for="patronymic">
            <BFormInput
              id="patronymic"
              type="text"
              v-model="formData.patronymic"
            />
          </BFormGroup>
        </BCol>
      </BRow>

      <BRow>
        <BCol cols="4">
          <BFormGroup label="Дата рождения" label-for="datebirth">
            <BFormInput
              id="datebirth"
              type="text"
              placeholder="дд.мм.гггг"
              v-model="formData.dateOfBirth"
            />
          </BFormGroup>
        </BCol>
      </BRow>

      <BRow>
        <BCol cols="9">
          <BFormGroup label="E-mail" label-for="email">
            <BFormInput
              id="email"
              type="email"
              v-model="formData.email"
              placeholder="example@example.com"
            />
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
                id="passportSeries"
                type="text"
                v-model="formData.passportSeries"
              />
            </BFormGroup>
          </BCol>
          <BCol cols="3">
            <BFormGroup label="Номер паспорта" label-for="passportNumber">
              <BFormInput
                id="passportNumber"
                type="text"
                v-model="formData.passportNumber"
              />
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
                id="lastname_en"
                type="text"
                v-model="formData.lastName_en"
              />
            </BFormGroup>
          </BCol>

          <BCol cols="5">
            <BFormGroup label="Имя на латинице" label-for="firsname_en">
              <BFormInput
                id="firsname_en"
                type="text"
                v-model="formData.firstName_en"
              />
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
                id="foreignPassportNumber"
                type="text"
                v-model="formData.foreignPassportNumber"
              />
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
                id="beforeChangeLastName"
                type="text"
                v-model="formData.beforeChangeLastName"
              />
            </BFormGroup>
          </BCol>

          <BCol cols="5">
            <BFormGroup
              label="Предыдущее имя"
              label-for="beforeChangeFirstName"
            >
              <BFormInput
                id="beforeChangeFirstName"
                type="text"
                v-model="formData.beforeChangeFirstName"
              />
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
      let res = this.allCountries.filter((country) => {
        if (
          country.nationality
            .toLowerCase()
            .startsWith(this.searchCountry.toLowerCase())
        ) {
          return country;
        }
      });
      return res;
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
      console.log(this.formData);
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
</style>
