<template>
  <modal
      title="Modal with form"
      @close="closeModal"
  >
    <!-- body -->
    <div slot="body">
      <form @submit.prevent="onSubmit">

        <!-- name -->
        <div class="form-item" :class="{ errorInput: $v.name.$error }">
          <label>Name: </label>
          <p class="errorText" v-if="!$v.name.required">Filed is required!</p>
          <p class="errorText" v-if="!$v.name.minLength">
            Name must have at least {{ $v.name.$params.minLength.min }}!
          </p>
          <input
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
              v-model="email"
              :class="{ error: $v.email.$error }"
              @blur="$v.email.$touch()">
        </div>

        <!-- button -->
        <button type="submit" class="btn btnPrimary">Submit!</button>

      </form>
    </div>
  </modal>
</template>

<script>
import {required, minLength, email} from 'vuelidate/lib/validators'
import Modal from "@/components/UI/Modal";

export default {
  name: "ModalValidate",
  components: {
    Modal,
  },
  data() {
    return {
      name: '',
      email: '',
    }
  },
  validations: {
    name: {
      required,
      minLength: minLength(4)
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
          name: this.name,
          email: this.email
        };
        console.log(user);
        // DONE !!!
        this.name= '';
        this.email= '';
        this.$v.$reset();
        this.$emit('close');
      }
    },
    closeModal(){
      this.name= '';
      this.email= '';
      this.$v.$reset();
      this.$emit('close');
    }
  }
}
</script>

<style lang="scss" scoped>
.errorText {
  display: none;
  margin-bottom: 8px;
  font-size: 14px;
  color: #ff0000;
}

.errorInput {
  .errorText {
    display: block;
  }
}

input {
  &.error {
    border-color: #ff0000;
  }
}
</style>