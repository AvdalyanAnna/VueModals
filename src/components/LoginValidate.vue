<template>
  <modal
      title="Log In"
      @close="closeModal"
  >
    <!-- body -->
    <div slot="body">
      <form @submit.prevent="onSubmit">

        <!-- password -->
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

        <!-- Repeat Password -->
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

        <!-- button -->
        <button type="submit" class="btn btnPrimary">Submit!</button>
        <a href="#" @click.prevent="$emit('open')"> Go to register </a>

      </form>
    </div>
  </modal>
</template>

<script>
import {required, minLength, email} from 'vuelidate/lib/validators'
import Modal from "@/components/UI/Modal";

export default {
  name: "LoginValidate",
  components: {
    Modal,
  },
  data() {
    return {
      email:'',
      password: '',
      showPassword: false,
    }
  },
  validations: {
    password: {
      required,
      minLength: minLength(6)
    },
    email: {
      required,
      email
    }

  },
  methods: {
    onSubmit() {
      this.$v.$touch();
      if (!this.$v.$invalid) {
        const user = {
          email: this.email,
          password: this.password
        };
        console.log(user);

        // DONE !!!
        this.password = '';
        this.email = '';
        this.showPassword = false;
        this.$v.$reset();
        this.$emit('close');

      }
    },
    closeModal() {
      this.password = '';
      this.email = '';
      this.showPassword = false;
      this.$v.$reset();
      this.$emit('close');
    }
  }
}
</script>

<style lang="scss">
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