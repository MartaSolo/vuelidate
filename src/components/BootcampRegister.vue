<script>
import { useVuelidate } from "@vuelidate/core";
import {
  required,
  email,
  minLength,
  maxLength,
  numeric,
} from "@vuelidate/validators";

const ensures10digit = (value) => value.trim().length === 10;

export default {
  setup: () => ({ v$: useVuelidate() }),
  data() {
    return {
      bootcamp: {
        name: "",
        email: "",
        location: "",
        phone: "",
      },
      feedback: "",
    };
  },
  methods: {
    async registerBootcamp() {
      this.v$.$touch();
      const isFormValid = await this.v$.$validate();
      if (isFormValid) {
        console.log("Your form was successfully submitted");
        console.log(this.bootcamp);
      } else {
        return;
      }
    },
  },
  validations() {
    return {
      bootcamp: {
        name: {
          required,
          minLength: minLength(3),
          maxLength: maxLength(30),
        },
        email: { required, email },
        location: { required },
        phone: {
          required,
          numeric,
          ensures10digit,
        },
      },
    };
  },
};
</script>

<template>
  <div class="bootcamp-register container">
    <form
      class="card-panel green-text"
      @submit.prevent="registerBootcamp"
      novalidate
    >
      <h2 class="center">Register form</h2>

      <div class="field">
        <label for="name">Name</label>
        <input
          id="name"
          type="text"
          v-model="bootcamp.name"
          placeholder="Enter the name"
          @blur="v$.bootcamp.name.$touch()"
        />
        <template v-if="v$.bootcamp.name.$error">
          <p class="errorMessage" v-if="v$.bootcamp.name.required.$invalid">
            Name is required
          </p>
          <p class="errorMessage" v-if="v$.bootcamp.name.minLength.$invalid">
            Name is has to be at least 3 characters
          </p>
          <p class="errorMessage" v-if="v$.bootcamp.name.maxLength.$invalid">
            Name cannot be longer then 30 characters
          </p>
        </template>
      </div>

      <div class="field">
        <label for="email">Email</label>
        <input
          id="email"
          type="email"
          v-model="bootcamp.email"
          placeholder="Enter the email"
          @blur="v$.bootcamp.email.$touch"
        />
        <template v-if="v$.bootcamp.email.$error">
          <p class="errorMessage" v-if="v$.bootcamp.email.required.$invalid">
            Email is required
          </p>
          <p class="errorMessage" v-if="v$.bootcamp.email.email.$invalid">
            Email is invalid
          </p>
        </template>
      </div>

      <div class="field">
        <label for="location">Location</label>
        <input
          id="location"
          type="text"
          v-model="bootcamp.location"
          placeholder="Enter the phone number"
          @blur="v$.bootcamp.location.$touch"
        />
        <template v-if="v$.bootcamp.location.$error">
          <p class="errorMessage" v-if="v$.bootcamp.location.required.$invalid">
            Location is required
          </p>
        </template>
      </div>

      <div class="field">
        <label for="phone">Phone number</label>
        <input
          id="phone"
          type="text"
          v-model="bootcamp.phone"
          placeholder="Enter the phone number"
          @blur="v$.bootcamp.phone.$touch"
        />
        <template v-if="v$.bootcamp.phone.$error">
          <p class="errorMessage" v-if="v$.bootcamp.phone.required.$invalid">
            Phone number is required
          </p>
          <p class="errorMessage" v-if="v$.bootcamp.phone.numeric.$invalid">
            Only digits are allowed
          </p>
          <p
            class="errorMessage"
            v-if="v$.bootcamp.phone.ensures10digit.$invalid"
          >
            Please enter 10 digit phone number
          </p>
        </template>
      </div>

      <div class="field center">
        <button class="btn green" type="submit" :disabled="v$.$invalid">
          Register
        </button>
      </div>
    </form>
  </div>
</template>

<style scoped>
.bootcamp-register {
  max-width: 600px;
  margin-top: 60px;
}
.bootcamp-register h2 {
  font-size: 2.4rem;
}
.bootcamp-register .field {
  margin-bottom: 16px;
}
.errorMessage {
  color: rgb(236, 79, 79);
}
</style>
