<template>
  <div class="container mt-5">
    <form @submit.prevent="handleSubmit">
      <div class="mb-3">
        <label for="country" class="form-label">Selecciona un país:</label>
        <select v-model="selectedCountry" @change="checkOtherCountry" class="form-select">
          <option value="Mexico">México</option>
          <option value="USA">Estados Unidos</option>
          <option value="Germany">Alemania</option>
          <option value="other">Otro</option>
        </select>
      </div>

      <div v-if="showOtherCountryInput" class="mb-3">
        <label for="otherCountry" class="form-label">Especifica otro país:</label>
        <input type="text" v-model="otherCountry" id="otherCountry" class="form-control" />
      </div>

      <button type="submit" class="btn btn-primary">Enviar</button>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      selectedCountry: '',
      showOtherCountryInput: false,
      otherCountry: '',
    };
  },
  methods: {
    checkOtherCountry() {
      this.showOtherCountryInput = this.selectedCountry === 'other';
      if (!this.showOtherCountryInput) {
        this.otherCountry = '';
      }
    },
    handleSubmit() {
      const country = this.selectedCountry === 'other' ? this.otherCountry : this.selectedCountry;
      alert(`País seleccionado: ${country}`);
    },
  },
};
</script>

<style scoped>
/* Aquí puedes agregar estilos adicionales si es necesario */
</style>
