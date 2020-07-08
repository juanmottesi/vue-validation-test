/* eslint-disable vue/require-v-for-key */
<template>
  <div>
    <div v-for="(v, index) in $v.people.$each.$iter" :key="index">
      <div  class="form-group" :class="{ 'form-group--error': v.$error }">
        <label class="form__label">Name for {{ index }}</label>
        <input class="form__input" v-model.trim="v.name.$model" />
      </div>
      <div class="form-group" :class="{ 'form-group--error': v.$error }">
          <label class="form__label">Age for {{ index }}</label>
          <input class="form__input" v-model="v.age.$model" />
      </div>
      <div class="error" v-if="!v.name.required">Name is required.</div>
      <div class="error" v-if="!v.name.minLength">
        Name must have at least {{ v.name.$params.minLength.min }} letters.
      </div>
    </div>
    <div>
      <button class="button" @click="people.push({ name: '', age: '' })">Add</button>
      <button class="button" @click="people.pop()">Remove</button>
    </div>
    <div
      class="form-group"
      :class="{ 'form-group--error': $v.people.$error }"
    ></div>
    <div class="error" v-if="!$v.people.required">
      List must not be empty.
    </div>
    <div class="error" v-else-if="$v.people.$error">List is invalid.</div>
    <button class="button" @click="$v.people.$touch">$touch</button>
    <button class="button" @click="$v.people.$reset">$reset</button>
  </div>
</template>

<script>
import { required, minLength } from "vuelidate/lib/validators";
export default {
  data() {
    return {
      people: [
        {
          name: "John",
          age: "",
        },
      ],
    };
  },
  validations: {
    people: {
      required,
      $each: {
        name: {
          required,
          minLength: minLength(2),
        },
        age: {
          required,
          minLength: minLength(1),
        }
      },
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
