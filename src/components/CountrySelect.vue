<template>
  <select
    @change="chooseCountry"
    v-model="selected"
    class="form-select mt-10 block w-full border p-3 rounded"
  >
    <option value="">Select Country or Global</option>
    <option value="1">Global</option>
    <option v-for="country in countries" :value="country.ID" :key="country.ID">
      {{ country.Country }}
    </option>
  </select>
</template>

<script>
export default {
  name: "CountrySelect",
  props: ["countries"],
  data() {
    return {
      selected: "",
    };
  },
  methods: {
    chooseCountry() {
      this.$emit("getSelectedCoutryID", this.selected);
    },
  },
};
</script>

<style></style>
