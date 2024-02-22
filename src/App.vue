<template>
  <div>
    <form @submit.prevent="submitForm">
      <label for="name">Nombre: </label>
      <input type="text" id="name" v-model="formData.name" required />
      <div ref="container" class="frc-captcha" data-sitekey="FCMJGJDK4IBR2AJA" data-lang="es"></div>
      <button type="submit">Enviar</button>
    </form>
  </div>
</template>

<script>
import { WidgetInstance } from "friendly-challenge";
import { ref } from "vue";
import CaptchaService from './services/CaptchaResponse'

export default {
  data() {
    return {
      container: ref("container"),
      widget: ref(),
      formData: {
        name: "",
        captchaToken: null, //token
      },
    };
  },
  methods: {
    submitForm: () => { },

    async verifyCaptcha(solution) {
      let response = await CaptchaService.
      verificarCaptcha(solution);
      console.log(response);
    },  
    doneCallback(solution) {
     this.verifyCaptcha(solution);
    },

    errorCallback: (err) => {
      console.log("There was an error when trying to solve the Captcha.");
      console.log(err);
    },
  },
  mounted() {
    if (this.$refs.container) {
      this.widget = new WidgetInstance(this.$refs.container, {
        // startMode: 'auto',
        doneCallback: this.doneCallback,
        errorCallback: this.errorCallback,
      });
    }
  },
  beforeDestroy() {
    if (this.widget) {
      this.widget.destroy();
    }
  },
};
</script>
<style></style>
