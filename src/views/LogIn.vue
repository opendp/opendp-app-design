<template>
  <div class="log-in mt-5">
    <v-container>
      <v-row>
        <v-col
          offset-md="2"
          offset-sm="1"
          sm="8"
          md="5"
          :class="{ 'px-10': $vuetify.breakpoint.xsOnly }"
        >
          <h1 class="title-size-1">Log in</h1>
          <v-form v-model="validLoginForm">
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
              @click:append="showPassword = !showPassword"
            ></v-text-field>
            <router-link
              :to="NETWORK_CONSTANTS.FORGOT_YOUR_PASSWORD.PATH"
              class="text-decoration-none"
              >Forgot your password?</router-link
            >
            <div class="mt-14 mb-7">
              <Button
                :class="{
                  'mb-2 width80 mx-auto d-block': $vuetify.breakpoint.xsOnly,
                  'mr-2': $vuetify.breakpoint.smAndUp
                }"
                color="primary"
                :disabled="!validLoginForm"
                :click="handleLogin"
                label="Log in"
              />
              <Button
                color="primary"
                :class="{
                  'width80 mx-auto d-block': $vuetify.breakpoint.xsOnly
                }"
                outlined
                :click="() => $router.push(NETWORK_CONSTANTS.HOME.PATH)"
                label="Cancel"
              />
            </div>
          </v-form>
          <div
            class="my-5"
            :class="{
              'text-center': $vuetify.breakpoint.xsOnly
            }"
          >
            <span
              >New user?
              <router-link
                :to="NETWORK_CONSTANTS.SIGN_UP.PATH"
                class="text-decoration-none font-weight-bold"
                >Create account</router-link
              ></span
            >
          </div>
          <div class="mb-10 social-login">
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

<script>
import SocialLoginButton from "../components/Accounts/SocialLoginButton.vue";
import SocialLoginSeparator from "../components/Accounts/SocialLoginSeparator.vue";
import Button from "../components/DesignSystem/Button.vue";
import NETWORK_CONSTANTS from "../router/NETWORK_CONSTANTS";
export default {
  name: "MyData",
  components: { SocialLoginButton, SocialLoginSeparator, Button },
  methods: {
    handleLogin: function() {
      // TODO: In this method you should implement the actual login logics
      localStorage.setItem("isLoggedUser", true);
      this.$router.push(NETWORK_CONSTANTS.WELCOME.PATH);
    },
    loginGoogle: function() {
      //TODO: Implement Login with Google
      alert("login with Google!");
    },
    loginGithub: function() {
      //TODO: Implement Login with Github
      alert("login with Github!");
    }
  },
  data: () => ({
    validLoginForm: false,
    showPassword: false,
    email: "",
    password: "",
    emailRules: [
      v => !!v || "E-mail is required",
      v => /.+@.+\..+/.test(v) || "E-mail must be valid"
    ],
    passwordRules: [v => !!v || "Password is required"],
    NETWORK_CONSTANTS
  })
};
</script>
