<template>
  <modalComponent title="Please sign up to continue" @close="closeForm">
    <!-- body -->
    <template v-slot:body>
      <form @submit.prevent="onSubmit">

        <!-- name -->
        <div class="form-item" :class="{ errorInput: v$.name.$error }">
          <label>Name:</label>
          <p class="errorText" v-if="!v$.name.$minLength">The name must have at least 4 letters</p>
          <input
            v-model="name"
            :class="{ error: v$.name.$error }"
            @change="v$.name.$touch()"/>
        </div>

        <!-- email -->
        <div class="form-item" :class="{ errorInput: v$.email.$error }">
          <label>Email:</label>
          <p class="errorText" v-if="!v$.email.$email">Email is not correct!</p>
          <input
            v-model="email"
            :class="{ error: v$.email.$error }"
            @change="v$.email.$touch()"/>
        </div>

        <!-- password -->
        <div class="form-item" :class="{ errorInput: v$.password.$error }">
          <label>Password:</label>
          <p class="errorText" v-if="!v$.password.$minLength">Password must have at least 8 symbols</p>
          <input class="inputpas"
            type="password"
            v-model="password"
            v-show="!showPass"
            :class="{ error: v$.password.$error }"
            @change="v$.password.$touch()"/>
            <input type="text" v-model="password" v-show="showPass" :class="{ error: v$.password.$error }">
            <span class="showText" @click="showPass = !showPass">(. )( .)</span>
        </div>

        <!-- Confirm the password -->
        <div class="form-item" :class="{ errorInput: v$.dablpassword.$error }">
          <label>Confirm the password:</label>
          <p class="errorText" v-if="!v$.dablpassword.$required">This field is required! Duplicate your password</p>
          <input class="inputpas"
            type="password"
            v-model="dablpassword"
            v-show="!showDabl"
            :class="{ error: v$.dablpassword.$error }"
            @change="v$.dablpassword.$touch()"/>
            <input type="text" v-model="dablpassword" v-show="showDabl" :class="{ error: v$.dablpassword.$error }">
          <span class="showText" @click="showDabl = !showDabl">(. )( .)</span>
        </div>

        <!-- button -->
        <button class="btn btnPrimary">Submit!</button>

        <!-- to come in -->
        <div class="toComeIn">
          <p>I already have an account.<ins @click="openModalValidate">To come in</ins></p>
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
  name: "modalSignUpComponent",
  components: { modalComponent },
  setup() {
    return { v$: useVuelidate() };
  },
  data() {
    return {
      name: "",
      email: "",
      password: "",
      dablpassword: "",
      showPass: false,
      showDabl: false,
    };
  },
  validations() {
    return {
      name: { required, minLength: minLength(4) },
      email: { required, email },
      password: { required, minLength: minLength(8) },
      dablpassword: { required, minLength: minLength(8) },
    };
  },
  methods: {
    onSubmit() {
      this.v$.$touch();
      if (!this.v$.$invalid && this.password == this.dablpassword) {
        const users = {
          name: this.name,
          email: this.email,
          password: this.password,
          dablpassword: this.dablpassword
        } 
        console.log(users)
      
        this.name = '',
        this.email = '',
        this.password = '',
        this.dablpassword = '',
        this.v$.$reset(),
        this.$emit('close')
      }
    },  
      closeForm() {
      (this.name = ""),
        (this.email = ""),
        (this.password = ""),
        (this.dablpassword = ""),
        this.v$.$reset(),
        this.$emit("close");
    },
    openModalValidate() {
      this.$emit('openModalValidate')
    },
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
span {
  cursor: pointer;
}
.toComeIn {
  font-size: 15px;
  text-align: center;
  margin: 15px;
  
}
ins {
  cursor: pointer;
}

</style>