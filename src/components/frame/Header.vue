<template>
  <div id="Header">
    <b-navbar fixed="top" toggleable="lg" type="light" variant="light">
      <!-- <b-navbar-brand href="#" id="title">UGrab</b-navbar-brand> -->
      <b-card-img :src="require('assets/header.png')" class="rounded-0" id="userIcon"></b-card-img>
      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>
      <b-collapse id="nav-collapse" is-nav>
        <b-navbar-nav class="ml-auto">
          <b-form-input
            class="input"
            v-show="$route.name ==='HomePage' || $route.path === '/' ? true : false"
            v-model="form.email"
            type="email"
            required
            placeholder="Email"
          ></b-form-input>&nbsp;&nbsp;&nbsp;
          <b-form-input
            class="input"
            v-show="$route.name ==='HomePage' || $route.path === '/' ? true : false"
            v-model="form.password"
            type="password"
            required
            placeholder="Password"
          ></b-form-input>&nbsp;&nbsp;&nbsp;
          <b-button :disabled="!inputEnable" v-on:click="onSubmit" v-show="$route.name ==='HomePage' || $route.path === '/' ? true : false" id="login">Login</b-button>

          <div variant="link" v-show="$route.path ==='/dashboard' || $route.path === '/personalinformation' || $route.path === '/copyProfile'|| $route.path === '/profileUser' || $route.path === '/authorizationForm' || $route.path === '/dashboardPartneredUser' || $route.path === '/viewAuth' ? true : false">
            <span
              style="margin-top:7px;color:#3483eb; cursor: pointer"
              v-on:click="redirect()"
              class="fa fa-fw fa-home fa-2x"
            ></span>
          </div>
          <div v-show="$route.path ==='/dashboard' || $route.path === '/personalinformation' || $route.path === '/copyProfile'|| $route.path === '/profileUser' || $route.path === '/authorizationForm' || $route.path === '/dashboardPartneredUser' || $route.path === '/viewAuth' ? true : false">
            <b-dropdown variant="link" toggle-class="text-decoration-none" no-caret>
              <template v-slot:button-content>
                <span class="fas fa-user-circle fa-2x"></span>
              </template>
              <b-dropdown-item v-on:click="goToPersonalInfo()">View Profile</b-dropdown-item>
              <b-dropdown-item v-on:click="logout()">Logout</b-dropdown-item>
            </b-dropdown>
          </div>
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>
  </div>
</template>

  <script>
import ROUTER from "router";
import AUTH from "services/auth";
export default {
  data() {
    return {
      inputEnable: true,
      user: "",
      auth: AUTH,
      form: {
        email: "",
        password: ""
      },
      userEmail: null
    };
  },
  methods: {
    goToPersonalInfo(){
      if(localStorage.getItem("partner")){
        ROUTER.push('/copyProfile')
      }else{
        ROUTER.push('/profileUser')
      }
    },
    redirect() {
      if(localStorage.getItem("partner")){
        ROUTER.push('/dashboardPartneredUser')
      }else{
        ROUTER.push('/dashboard')
      }
    },

    logout(){
      AUTH.logout()
    },

    onSubmit(e) {
      e.preventDefault();
      AUTH.login(this.form.email, this.form.password)
      this.form.email = ""
      this.form.password = ""
    }
  }
};
</script>

<style scoped lang="scss">
@import "~assets/color.scss";
#userIcon {
  width: 7%;
  height: auto;
}
#login {
  background: $motif;
  height: 39px;
}
.input {
  border-color: $motif;
}
a {
  font-size: 20px;
}
</style>
