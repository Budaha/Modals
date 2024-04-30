<template>
  <div class="wrapper">
    <div class="wrapper-content">
      <section>
        <div class="container">

          <!-- first modal -->
          <button class="btn btnPrimary" @click="onButtonClick">Show first modal</button>
          <modalComponent
            title="First modal"
            v-show="modalFirst"
            @close="closeModalFirst">
            <!-- body -->
            <template v-slot:body>
              <p>Text Text Text</p>
              <button class="btn btnPrimary" @click="onButtonClick">Well Done</button>
            </template>
          </modalComponent>

          <!-- second modal -->
          <button class="btn btnPrimary" @click="oneButtonClick">Show modal with form</button>
          <modalComponent
            title="Modal with form"
            v-show="modalSecond.show"
            @close="closeModalSecond">
            <!-- body -->
            <template v-slot:body>
              <form @submit.prevent="submitSecondForm">
                <label>Name:</label>
                <input type="text" required v-model="modalSecond.name" />
                <label>Email:</label>
                <input type="email" required v-model="modalSecond.email" />
                <button class="btn btnPrimary">Submit!</button>
              </form>
            </template>
          </modalComponent>

          <!-- modal with Validate -->
          <button class="btn btnPrimary" @click="twoButtonClick">Show modal with form + Validate</button>
          <modalValidateComponent
            v-show="modalValidate"
            @close="closeModalValidate"
            @openModalSignUp="openModalSignUp"/>

          <!-- Sign Up -->
          <button class="btn btnPrimary" @click="threeButtonClick">Sign Up</button>
          <modalSignUpComponent
            v-show="modalSignUp"
            @close="closeModalSignUp"
            @openModalValidate="openModalValidate"/>
        </div>
      </section>
    </div>
  </div>
</template>

<script>
import modalComponent from "@/components/UI/Modal.vue";
import modalValidateComponent from "@/components/ModalValidate.vue";
import modalSignUpComponent from "./components/ModalSignUpComponent.vue";
export default {
  components: {
    modalComponent,
    modalValidateComponent,
    modalSignUpComponent,
  },
  data() {
    return {
      modalFirst: false,
      modalSecond: {
        show: false,
        name: "",
        email: "",
      },
      modalValidate: false,
      modalSignUp: false,
    };
  },
  methods: {
    onButtonClick() {
      this.modalFirst = !this.modalFirst;
    },
    closeModalFirst() {
      this.modalFirst = false;
    },
    oneButtonClick() {
      this.modalSecond.show = !this.modalSecond.show;
    },
    closeModalSecond() {
      (this.modalSecond.name = ""),
        (this.modalSecond.email = ""),
        (this.modalSecond.show = false);
    },
    submitSecondForm() {
      console.log({
        name: this.modalSecond.name,
        email: this.modalSecond.email,
      });
      this.modalSecond.name = "";
      this.modalSecond.email = "";
      this.modalSecond.show = false;
    },
    twoButtonClick() {
      this.modalValidate = !this.modalValidate;
    },
    closeModalValidate() {
      this.modalValidate = false
    },
    threeButtonClick() {
      this.modalSignUp = !this.modalSignUp
    },
    closeModalSignUp() {
      this.modalSignUp = false
    },
    openModalSignUp() {
      this.modalValidate = false,
      this.modalSignUp = true
    },
    openModalValidate() {
      this.modalSignUp = false,
      this.modalValidate = true
    }
  },
};
</script>