<template>
    <modal-component
        title="Modal with form + Validate"
        @close="$emit('close')">
        <!--body-->
        <template v-slot:body>
            <form @submit.prevent="onSubmit">
            <!-- name -->
            <div class="form-item" :class="{ errorInput: v$.name.$error }">   
                <label>Name:</label>
                <p class="errorText" v-if="!v$.name.$required"> Filed is required! </p>
                <p class="errorText" v-if="!v$.name.$minLength"> Name must have at least 4 </p>
                <input type="text" v-model="name"
                 :class="{error: v$.name.$error}"
                 @change="v$.name.$touch()">
            </div>
            <!--email-->
            <div class="form-item" :class="{ errorInput: v$.email.$error }">
                <label>Email:</label>
                <p class="errorText" v-if="!v$.email.$required"> Filed is required! </p>
                <p class="errorText" v-if="!v$.email.$email"> Email is not correct! </p>
                <input type="email" v-model="email"
                :class="{ error: v$.email.$error }"
                @change="v$.email.$touch()">
            </div>
            <!-- button -->
            <button class="btn btnPrimary" @click="submitForm">Submit!</button>
            </form>
        </template>
    </modal-component>
</template>

<script>
import { useVuelidate } from '@vuelidate/core'
import { required, email, minLength } from '@vuelidate/validators'
import modalComponent from '@/components/UI/Modal.vue'
export default {
    name: "modalValidate",
    components: {modalComponent},
    data () {
        return {
            v$: useVuelidate(),
            name: '',
            email: ''
        }
    },
    validations() { 
        return {
        name: { required, minLength: minLength(4) },
        email: { required, email }
        }
    },
    methods: {
        onSubmit () {
            this.v$.$touch()
            if (!this.v$.$invalid) {
                const user = {
                    name: this.name,
                    email: this.email
                }
                console.log(user)
                // Done
                this.name = ''
                this.email = ''
                this.v$.$reset()
                this.$emit('close')
            }
        },
        submitForm() {
           // console.log(this.v$)
            this.v$.$validate()
            if (!this.v$.$error) {
                alert('Form successfully submitted.')
            } else {
                alert ('Form failed validation.')
            }
        },
    }
}

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
</style>
