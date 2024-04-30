<template>
  <modalComponent title="Modal with form + Validate" @close="onClose">
    <!-- body -->
    <template v-slot:body>
      <form @submit.prevent="onSubmit">
        <!-- name -->
        <div class="form-item" :class="{ errorInput: v$.name.$error }">
          <label>Name:</label>
          <!-- <p class="errorText" v-if="!v$.name.$required">Filed is required!</p> -->
          <p class="errorText" v-if="!v$.name.$minLength">The name must have at least 4 letters</p>
          <input
            v-model="name"
            :class="{ error: v$.name.$error }"
            @change="v$.name.$touch()"/>
        </div>
        <!-- email -->
        <div class="form-item" :class="{ errorInput: v$.email.$error }">
          <label>Email:</label>
           <!-- <p class="errorText" v-if="!v$.email.$required">Filed is required!</p> -->
          <p class="errorText" v-if="!v$.email.$email">Email is not correct!</p>
          <input
            v-model="email"
            :class="{ error: v$.email.$error }"
            @change="v$.email.$touch()"/>
        </div>
        <!-- password -->
        <div class="form-item" :class="{ errorInput: v$.password.$error }">
          <label>Password:</label>
          <!-- <p class="errorText" v-if="!v$.password.$required">Filed is required!</p> -->
          <p class="errorText" v-if="!v$.password.$minLength">Password must have at least 8 symbols</p>
          <input
            type="password"
            v-model="password"
            :class="{ error: v$.password.$error }"
            @change="v$.password.$touch()"/>
        </div>
        <!-- button -->
        <button class="btn btnPrimary">Submit!</button>
        <!-- sign up -->
        <div class="signup">
          <p>Don't have an account?<ins @click="openModalSignUp">Sign Up</ins></p>
        </div>
      </form>
    </template>
  </modalComponent>
</template>

<script>
import { useVuelidate } from "@vuelidate/core";
import { required, email, minLength } from "@vuelidate/validators";
import modalComponent from "@/components/UI/Modal.vue";

export default {
  name: "modalValidateComponent",
  components: { modalComponent },
  setup() {
    return { v$: useVuelidate() };
  },
  data() {
    return {
      name: "",
      email: "",
      password: "",
    };
  },
  validations() {
    return {
      name: { required, minLength: minLength(4) },
      email: { required, email },
      password: {required, minLength: minLength(8)}
    };
  },
  methods: {
    onSubmit () {
        this.v$.$touch()
        if (!this.v$.$invalid) {
            const user = {
                name: this.name,
                email: this.email,
                password: this.password,
            }
            console.log(user)
            this.name = '',
            this.email = '',
            this.password = '',
            this.v$.$reset(),
            this.$emit('close')
        }
    },
    onClose() {
        this.name = '',
        this.email = '',
        this.password = '',
        this.v$.$reset(),
        this.$emit('close')
        this.modalValidate = false
    },
    openModalSignUp() {
      this.$emit('openModalSignUp')
    }
  }
};
</script>

<style lang="scss">
.form-item {
  p {
    color: red;
  }
}
.form-item .errorText {
  display: none;
  margin-bottom: 8px;
  font-size: 13.4px;
  color: #de4040;
  margin: 10px;
}
.form-item {
  &.errorInput .errorText {
    display: block;
  }
}
input.error {
  border-color: #de4040;
}
.signup {
  font-size: 15px;
  text-align: center;
  margin: 15px;
  
}
ins {
  cursor: pointer;
}
</style>