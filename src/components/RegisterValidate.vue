<template>
  <modal
      title="Registration"
      @close="closeModal"
  >
    <!-- body -->
    <div slot="body">
      <form @submit.prevent="onSubmit">

        <!-- login -->
        <div class="form-item" :class="[{ errorInput: $v.login.$error }]">
          <label>Login: </label>
          <p class="errorText" v-if="!$v.login.required">Filed is required!</p>
          <p class="errorText" v-if="!$v.login.minLength">
            Login must have at least {{ $v.login.$params.minLength.min }}!
          </p>
          <input
              type="text"
              v-model="login"
              :class="{ error: $v.login.$error }"
              @blur="$v.login.$touch()">
        </div>

        <!-- name -->
        <div class="form-item" :class="[{ errorInput: $v.name.$error }]">
          <label>Name: </label>
          <p class="errorText" v-if="!$v.name.required">Filed is required!</p>
          <p class="errorText" v-if="!$v.name.minLength">
            Name must have at least {{ $v.name.$params.minLength.min }}!
          </p>
          <input
              type="text"
              v-model="name"
              :class="{ error: $v.name.$error }"
              @blur="$v.name.$touch()">
        </div>

        <!-- email -->
        <div class="form-item" :class="{ errorInput: $v.email.$error }">
          <label>Email:</label>
          <p class="errorText" v-if="!$v.email.required">Filed is required!</p>
          <p class="errorText" v-if="!$v.email.email">Email is not correct! </p>
          <input
              type="email"
              v-model="email"
              :class="{ error: $v.email.$error }"
              @blur="$v.email.$touch()">
        </div>
        <!-- password -->
        <div class="form-item password" :class="[{ errorInput: $v.password.$error },{ active: showPassword }]">
          <label>Password: </label>
          <p class="errorText" v-if="!$v.password.required">Filed is required!</p>
          <p class="errorText" v-if="!$v.password.minLength">
            Password must have at least {{ $v.password.$params.minLength.min }}!
          </p>
          <input
              :type="showPassword ? 'text' : 'password'"
              v-model="password"
              :class="{ error: $v.password.$error }"
              @blur="$v.password.$touch()">
          <span class="eye" @click="showPassword = !showPassword"></span>
        </div>

        <!-- Repeat Password -->
        <div class="form-item password" :class="[{ errorInput: $v.repeatPassword.$error },{ active: showRepeatPassword }]">
          <label>Repeat password:</label>
          <p class="errorText" v-if="!$v.repeatPassword.required">Filed is required!</p>
          <p class="errorText" v-if="!$v.repeatPassword.sameAsPassword">Repeat password is not correct! </p>
          <input
              :type="showRepeatPassword ? 'text' : 'password'"
              v-model="repeatPassword"
              :class="{ error: $v.repeatPassword.$error }"
              @blur="$v.repeatPassword.$touch()">
          <span class="eye" @click="showRepeatPassword = !showRepeatPassword"></span>
        </div>

        <!-- button -->
        <button type="submit" class="btn btnPrimary">Submit!</button>

        <a href="#" @click="$emit('open')">  Go to Login </a>
      </form>
    </div>
  </modal>
</template>

<script>
import {required, minLength, sameAs, email} from 'vuelidate/lib/validators'
import Modal from "@/components/UI/Modal";

export default {
  name: "PasswordValidate",
  components: {
    Modal,
  },
  data() {
    return {
      login:'',
      name: '',
      email: '',
      password: '',
      repeatPassword: '',
      showPassword: false,
      showRepeatPassword: false,
    }
  },

  validations: {
    login: {
      required,
      minLength: minLength(4)
    },
    name: {
      required,
      minLength: minLength(4)
    },
    email: {
      required,
      email
    },
    password: {
      required,
      minLength: minLength(6)
    },
    repeatPassword: {
      required,
      sameAsPassword: sameAs('password')
    }
  },
  methods: {
    onSubmit() {
      this.$v.$touch();
      if (!this.$v.$invalid) {
        const user = {
          login: this.login,
          name: this.name,
          email: this.email,
          password: this.password,
          repeatPassword: this.repeatPassword
        };
        console.log(user);
        // DONE !!!
        this.login = '';
        this.name = '';
        this.email = '';
        this.password = '';
        this.repeatPassword = '';
        this.showPassword = false;
        this.showRepeatPassword = false;
        this.$v.$reset();
        this.$emit('close');
      }
    },
    closeModal() {
      this.login = '';
      this.name = '';
      this.email = '';
      this.password = '';
      this.repeatPassword = '';
      this.showPassword = false;
      this.showRepeatPassword = false;
      this.$v.$reset();
      this.$emit('close');
    }
  }
}
</script>

<style lang="scss">
form{
  a{
    display: block;
    margin: 20px;
    font-size: 14px;
  }
}
.form-item {
  .errorText {
    display: none;
    margin-bottom: 8px;
    font-size: 14px;
    color: #ff0000;
  }

  &.errorInput {
    .errorText {
      display: block;
    }
  }

  &.password {
    position: relative;

    .eye {
      position: absolute;
      width: 25px;
      height: 25px;
      background-size: contain;
      background-image: url("/images/eye-slash-solid.svg");

      background-repeat: no-repeat;
      background-position: center;
      right: 15px;
      bottom: 45px;
      cursor: pointer;
    }

    &.active {
      .eye {
        background-image: url("/images/eye-solid.svg");
      }
    }
  }
}


input {
  &.error {
    border-color: #ff0000;
  }

}
</style>