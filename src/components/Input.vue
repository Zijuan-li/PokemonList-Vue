<template>
  <div>
    <input
      type="search"
      placeholder="Search Pokemon!"
      id="searchPoke"
      v-model="filterKeyword"
    />
    <label>Generation:</label>
    <select v-model="gen">
      <option value="1">I</option>
      <option value="2">II</option>
      <option value="3">III</option>
      <option value="4">IV</option>
      <option value="5">V</option>
      <option value="6">VI</option>
      <option value="7">VII</option>
      <option value="8">VIII</option>
    </select>
    <input type="checkbox" id="shinyCheckbox" v-model="isShiny" />
    <label htmlFor="shinyCheckbox">Shiny!</label>
  </div>
</template>

<script>
import { EventBus } from "../main.js";
export default {
  name: "Input",
  data() {
    return {
      filterKeyword: "",
      gen: "",
      isShiny: false,
    };
  },
  watch: {
    filterKeyword: function () {
      EventBus.$emit("update-filter", this.filterKeyword);
    },
    gen: function () {
      EventBus.$emit("update-gen", this.gen);
    },
    isShiny: function () {
      EventBus.$emit("update-shiny", this.isShiny);
    },
  },
};
</script>

<style scoped>
input {
  margin: 5px;
}
</style>>