<template>
  <div>
    <h2
      class="title-size-2 mb-4"
      :class="{ 'ml-5': $vuetify.breakpoint.xsOnly }"
    >
      <strong>2/2. </strong>Sign up
    </h2>
    <v-container class="ml-0">
      <v-row>
        <v-col
          md="8"
          class="pl-0"
          :class="{ 'ml-5': $vuetify.breakpoint.xsOnly }"
        >
          <v-form
            v-model="validSignUpForm"
            ref="signUpForm"
            @submit.prevent="handleFormSubmit"
          >
            <v-text-field
              v-model="username"
              label="Username"
              :rules="usernameRules"
              required
            ></v-text-field>
            <v-text-field
              v-model="email"
              label="Email"
              required
              :rules="emailRules"
              type="email"
            ></v-text-field>
            <v-text-field
              v-model="password"
              :append-icon="showPassword ? 'mdi-eye' : 'mdi-eye-off'"
              :type="showPassword ? 'text' : 'password'"
              name="input-10-1"
              label="Password"
              :rules="passwordRules"
              counter
              @click:append="showPassword = !showPassword"
            ></v-text-field>
            <span
              id="password-hint"
              class="d-block grey--text text--darken-2 mb-5"
              :class="{ 'pl-6': $vuetify.breakpoint.smAndUp }"
              >Your <strong>password</strong> must be at least 6 characters long
              and must contain letters, numbers and special characters. Cannot
              contain whitespace.</span
            >
            <v-text-field
              v-model="confirmPassword"
              :append-icon="showConfirmPassword ? 'mdi-eye' : 'mdi-eye-off'"
              :type="showConfirmPassword ? 'text' : 'password'"
              :rules="[confirmPasswordVerification, ...confirmPasswordRules]"
              name="input-10-2"
              label="Confirm password"
              counter
              @click:append="showConfirmPassword = !showConfirmPassword"
            ></v-text-field>
            <Button
              classes="mt-5"
              color="primary"
              :class="{
                'width80 mx-auto d-block': $vuetify.breakpoint.xsOnly,
                'mr-2': $vuetify.breakpoint.smAndUp
              }"
              type="submit"
              :disabled="!validSignUpForm"
              label="Create account"
            />
            <Button
              color="primary"
              classes="mt-5"
              :class="{ 'width80 mx-auto d-block': $vuetify.breakpoint.xsOnly }"
              outlined
              :click="() => $router.push(NETWORK_CONSTANTS.HOME.PATH)"
              label="Cancel"
            />
          </v-form>
          <div class="mt-10 social-login">
            <SocialLoginSeparator />
            <SocialLoginButton
              mdiIcon="mdi-google"
              iconBgColor="#C53126"
              label="Log in with Google"
              labelBgColor="#F44336"
              :handler="loginGoogle"
            />
            <SocialLoginButton
              mdiIcon="mdi-github"
              iconBgColor="#2E2E2E"
              label="Log in with GitHub"
              labelBgColor="#050505"
              :handler="loginGithub"
            />
          </div>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<style lang="scss" scoped>
#password-hint {
  @media screen and (max-width: 600px) {
    font-size: 0.875rem;
  }
}
</style>

<script>
import Button from "../../DesignSystem/Button.vue";
import SocialLoginButton from "../SocialLoginButton.vue";
import SocialLoginSeparator from "../SocialLoginSeparator.vue";
import NETWORK_CONSTANTS from "../../../router/NETWORK_CONSTANTS";
export default {
  components: { SocialLoginButton, SocialLoginSeparator, Button },
  name: "SignUpForm",
  methods: {
    confirmPasswordVerification() {
      return this.password === this.confirmPassword || `Passwords don't match`;
    },
    checkIfNeedToAdjustPasswordConfirmation() {
      if (this.confirmPassword !== "") {
        this.$refs.signUpForm.validate();
      }
    },
    handleFormSubmit: function() {
      if (this.$refs.signUpForm.validate()) {
        //TODO: Implement Sign Up logic here
        this.$router.push(`${NETWORK_CONSTANTS.SIGN_UP.PATH}/confirmation`);
      }
    },
    loginGoogle: function() {
      //TODO: Implement Login with Google logic here
      alert("login with Google!");
    },
    loginGithub: function() {
      //TODO: Implement Login with Github logic here
      alert("login with Github!");
    }
  },
  data: () => ({
    validSignUpForm: false,
    showPassword: false,
    showConfirmPassword: false,
    username: "",
    email: "",
    password: "",
    confirmPassword: "",
    usernameRules: [v => !!v || "A username is required"],
    emailRules: [
      v => !!v || "E-mail is required",
      v => /.+@.+\..+/.test(v) || "E-mail must be valid"
    ],
    passwordRules: [
      v => {
        const pattern = /[\s]+/;
        return !pattern.test(v) || "Your password can't contain whitespaces";
      },
      v =>
        (v || "").length >= 6 ||
        "Your password has to be at least 6 characters long",
      v => {
        const pattern = /[a-zA-Z]+/;
        return (
          pattern.test(v) || "Your password has to have at least one letter"
        );
      },
      v => {
        const pattern = /[0-9]+/;
        return (
          pattern.test(v) || "Your password has to have at least one number"
        );
      },
      v => {
        const pattern = /[\W]+/;
        return (
          pattern.test(v) ||
          "Your password has to have at least one special character"
        );
      }
    ],
    confirmPasswordRules: [v => !!v || "Please confirm your password"],
    NETWORK_CONSTANTS
  })
};
</script>
