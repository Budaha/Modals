<template>
  <div class="wrapper">
    <div class="wrapper-content">

      <section>
        <div class="container">
          
          <!--first modal-->
          <button class="btn btnPrimary" @click="modalFirst = !modalFirst">Show first modal</button>
          <modals-component
              title="First modal"
              v-show="modalFirst"
              @close="modalFirst = false">
          <!--body-->
            <template v-slot:body>
              <p> Text Text Text </p>
              <button class="btn btnPrimary" @click="modalFirst = !modalFirst">Well Done!</button>
            </template>
          </modals-component>
          
          <!--second modal-->
          <button class="btn btnPrimary" @click="modalSecond.show = !modalSecond.show">Show modal with form</button>
          <modals-component
            title="Modal with form"
            v-show="modalSecond.show"
            @close="modalSecond.show = false">
          <!--body-->
          <template v-slot:body>
            <form @submit.prevent="submitSecondForm">
                <label>Name:</label>
                <input type="text" required v-model="modalSecond.name">
                <label>Email:</label>
                <input type="email" required v-model="modalSecond.email">
                <button class="btn btnPrimary">Submit!</button>
            </form>
          </template>
          </modals-component>

          <!--modal with validate-->
          <button class="btn btnPrimary" @click="modalValidate = !modalValidate">Show modal with form + validate</button>
          <modalValidate v-show="modalValidate" @close="modalValidate = false"/>

        </div>
      </section>

    </div>
   </div>
</template>

<script>
import modalsComponent from '@/components/UI/Modal.vue'
import modalValidate from '@/components/ModalValidate.vue'
export default {
  components: { modalsComponent, modalValidate },
  data() {
    return {
      modalFirst: false,
      modalSecond: {
        show: false,
        name: '',
        email: ''
      },
      modalValidate: false,
    }
  },
  methods: {
    submitSecondForm () {
      console.log({
        name: this.modalSecond.name,
        email: this.modalSecond.email
      })
      this.modalSecond.name = '',
      this.modalSecond.email = '',
      this.modalSecond.show = false
    }
  }
}
</script>



