<template>
  <div id="register" class="text-center">

    <section
      class="vh-100 bg-image"
      
    >
      <div class="mask d-flex align-items-center h-100 gradient-custom-3">
        <div class="container h-100">
          <div
            class="row d-flex justify-content-center align-items-center h-100"
          >
            <div class="col-12 col-md-9 col-lg-7 col-xl-6">
              <div class="card" style="border-radius: 15px">
                <div class="card-body p-5">
                  <h2 id="h2" class="text-uppercase text-center mb-5">
                    Create an account
                  </h2>
                  

                  <form id="form" @submit.prevent="registerForm">
                    <div role="alert" v-if="registrationErrors">
                      {{ registrationErrorMsg }}
                    </div>
                    <div class="form-outline mb-4">
                      <input
                        type="text"
                        name="user_name"
                        id="form3Example1cg"
                        class="form-control form-control-lg"
                        v-model="user.username"
                        required
                        autofocus
                      />
                      <label class="form-label" for="form3Example1cg"
                        >Username</label
                      >
                    </div>

                     <div class="form-outline mb-4">
                  <input type="email" id="form3Example3cg" class="form-control form-control-lg" name="to_email"/>
                  <label class="form-label" for="form3Example3cg">Email Address</label>
                </div> 
                    <div class="form-outline mb-4">
                      <input
                        type="password"
                        id="form3Example4cg"
                        class="form-control form-control-lg"
                        v-model="user.password"
                        required
                      />
                      <label class="form-label" for="form3Example4cg"
                        >Password</label
                      >
                    </div>

                    <div class="form-outline mb-4">
                      <input
                        type="password"
                        id="form3Example4cdg"
                        class="form-control form-control-lg"
                        v-model="user.confirmPassword"
                        required
                      />
                      <label class="form-label" for="form3Example4cdg"
                        >Confirm your Password</label
                      >
                    </div>

                    <div class="d-flex justify-content-center">
                      <button id="reg"
                        type="submit"
                        class="
                          btn btn-success btn-block btn-lg
                          gradient-custom-4
                          text-body btn-primary
                        "
                        value="Send" 
                      >
                        Register
                      </button>
                    </div>
                    <p class="form-label">
                      Already have an account?
                      <router-link :to="{ name: 'login' }">Log in.</router-link>
                    </p>
                  </form>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
      

  </div>
</template>

<script>


import authService from "../services/AuthService";
import emailjs from '@emailjs/browser';

export default {
  name: "register",
  
  data() {
    return {
      user: {
        username: "",
        password: "",
        confirmPassword: "",
        role: "user",
      },
      registrationErrors: false,
      registrationErrorMsg: "There were problems registering this user.",
    };
  },
  methods: {

    registerForm() {
      this.register();
      this.sendEmail();
    },

    sendEmail() {
      console.log('send email')
      emailjs.sendForm('service_rh1fri5', 'template_xthupel', '#form', 'KPvsLsZCDVoxWJbAQ')
        .then((result) => {
            console.log('SUCCESS!', result.text);
        }, (error) => {
            console.log('FAILED...', error.text);
        });
    },

    register() {
      if (this.user.password != this.user.confirmPassword) {
        this.registrationErrors = true;
        this.registrationErrorMsg = "Password & Confirm Password do not match.";
      } else {
        authService
          .register(this.user)
          .then((response) => {
            if (response.status == 201) {
              this.$router.push({
                path: "/login",
                query: { registration: "success" },
              });
            }
          })
          .catch((error) => {
            const response = error.response;
            this.registrationErrors = true;
            if (response.status === 400) {
              this.registrationErrorMsg = "Bad Request: Validation Errors";
            }
          });
      }
    },
    clearErrors() {
      this.registrationErrors = false;
      this.registrationErrorMsg = "There were problems registering this user.";
    },
  },
};
</script>

<style scoped>

.btn-primary, .btn {
    font-style: white;
    color:white;
}

.mb-5, .form-label {
  color: #232069
}

.form-input-group {
  margin-bottom: 1rem;
}

label {
  margin-right: 0.5rem;
}

#h2 {
  font-family: 'Lora', serif;
  font-weight: bold;
}

.vh-100 {
  background-image: url("https://res.cloudinary.com/dzw4n2mgp/image/upload/v1681593141/capstone_assets/resizehomepage_pdvyoo.png");
  background-color: transparent;
  justify-content: space-evenly;
  background-size: cover;
  height: 100vh;
}

.btn-success{
  background-color: #80AAFF;
  border-color: #80AAFF;
  color: white;
}

.btn-success:hover {
  background-color: #5990ff;
  border-color: #5990ff;
  color: white;
}

.btn-success:active {
  background-color: #5990ff;
  border-color: #5990ff;
  color: white;
  
}

.text-center {
  font-family: 'Poppins', sans-serif;
  font-weight: bold;

}

</style>
