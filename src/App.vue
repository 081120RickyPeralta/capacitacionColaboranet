<template>
  <div class="container mt-5">
    <form>
      <div class="mb-3">
        <label for="country" class="form-label">Selecciona un país:</label>
        <select v-model="selectedCountry" @change="checkOtherCountry" class="form-select">
          <option v-for="country in countries" :key="country.value" :value="country.value">
            {{ country.name }}
          </option>
        </select>
      </div>

      <div v-if="showOtherCountryInput" class="mb-3">
        <label for="otherCountry" class="form-label">Especifica otro país:</label>
        <input type="text" v-model="otherCountry" id="otherCountry" class="form-control" />
      </div>
    </form>
  </div>
</template>

<script>
import countriesData from './validacion.json';

export default {
  data() {
    return {
      selectedCountry: '',
      showOtherCountryInput: false,
      otherCountry: '',
      countries: countriesData.countries,
    };
  },
  methods: {
    checkOtherCountry() {
      const selected = this.countries.find((country) => country.value === this.selectedCountry);
      this.showOtherCountryInput = selected ? selected.requiresInput : false;
      if (!this.showOtherCountryInput) {
        this.otherCountry = '';
      }
    },
  },
};
</script>
