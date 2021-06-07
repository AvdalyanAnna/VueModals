<template>
  <div class="wrapper">
    <div class="wrapper-content">
      <section>
        <div class="container">

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
        </div>
      </section>
    </div>
  </div>
</template>

<script>
import Modals from "@/components/Modal";

export default {
  name: 'App',
  components: {
    Modals
  },
  data() {
    return {
      modalFirst: false,
      modalSecond: {
        show: false,
        name: '',
        email: '',
      }
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
    }
  }

}
</script>

<style lang="scss" scoped>

</style>
