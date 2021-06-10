<template>
  <div class="wrapper">
    <div class="wrapper-content">
      <section>
        <div class="container">
          <div class="d-flex">
            <!-- first modal -->
            <button class="btn btnPrimary" @click="modalFirst = !modalFirst">Show first modal</button>
            <modals
                title="First Modal"
                v-show="modalFirst"
                @close="modalFirst = false"
            >
              <!-- body -->
              <div slot="body">
                <p>Text Text Text Text Text Text Text Text Text </p>
                <button class="btn btnPrimary" @click="modalFirst = !modalFirst">Well Done!</button>
              </div>
            </modals>

            <!-- second modal -->
            <button class="btn btnPrimary" @click="modalSecond.show = !modalSecond.show">Show modal with form</button>
            <modals
                title="Modal with form"
                v-show="modalSecond.show"
                @close="modalSecond.show = false"
            >
              <!-- body -->
              <div slot="body">
                <form @submit.prevent="submitSecondForm">
                  <label>Name:</label>
                  <input type="text" required v-model="modalSecond.name">
                  <label>Email:</label>
                  <input type="email" required v-model="modalSecond.email">
                  <button type="submit" class="btn btnPrimary">Submit!</button>

                </form>
              </div>
            </modals>

            <!-- modal with validate  -->
            <button class="btn btnPrimary" @click="modalValidate = !modalValidate">Show modal with form validate
            </button>
            <modal-validate v-show="modalValidate" @close="modalValidate = false"/>

            <!-- modal with password validate  -->
            <button class="btn btnPrimary" @click="passwordValidate = !passwordValidate">Show modal with password
              validate
            </button>
            <password-validate v-show="passwordValidate" @close="passwordValidate = false"/>

            <!-- modal with register  -->
            <button class="btn btnPrimary" @click="registerValidate = !registerValidate">Show modal with Register
            </button>
            <register-validate
                v-show="registerValidate"
                @close="registerValidate = false"
                @open="loginValidate = true; registerValidate = false"/>

            <!-- modal with login  -->
            <button class="btn btnPrimary" @click="loginValidate = !loginValidate">Show modal with Login</button>
            <login-validate
                v-show="loginValidate"
                @close="loginValidate = false"
                @open="registerValidate = true; loginValidate = false"/>


          </div>
        </div>
      </section>
    </div>
  </div>
</template>

<script>
import Modals from "@/components/UI/Modal";
import ModalValidate from "@/components/ModalValidate";
import PasswordValidate from "@/components/PasswordValidate";
import RegisterValidate from "@/components/RegisterValidate";
import LoginValidate from "@/components/LoginValidate";

export default {
  name: 'App',
  components: {
    Modals,
    ModalValidate,
    PasswordValidate,
    RegisterValidate,
    LoginValidate
  },
  data() {
    return {
      modalFirst: false,
      modalSecond: {
        show: false,
        name: '',
        email: '',
      },
      modalValidate: false,
      passwordValidate: false,
      registerValidate: false,
      loginValidate: false,
    }
  },
  methods: {
    submitSecondForm() {
      console.log({
        name: this.modalSecond.name,
        email: this.modalSecond.email,
      })
      this.modalSecond.name = '';
      this.modalSecond.email = '';
      this.modalSecond.show = false;
    },
  }

}
</script>

<style lang="scss" scoped>
.d-flex {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  align-items: start;

  button {
    margin-bottom: 20px;
  }
}
</style>
