<template>
    <transition name="modal-fade">
        <div style="overflow: auto;">
            <div class="modal-backdrop">
                <div class="modal">
                    <header class="modal-header">
                        <h3>Register</h3>
                    </header>
                    <section class="modal-body w3-center">
                        <slot name="body">
                            <form class="w3-container w3-center" style="width: 100%" @submit.prevent="register">
                                <div class="w3-section">
                                    <div>
                                        <label style=""><b>Username</b></label><br>
                                        <input required v-model="username" type="text" placeholder="Username" class="w3-input w3-border w3-margin-bottom"/>
                                    </div>
                                    <div>
                                        <label style=""><b>Email</b></label><br>
                                        <input required v-model="email" type="text" placeholder="Email" class="w3-input w3-border w3-margin-bottom"/>
                                    </div>
                                    <div>
                                        <label><b>Name</b></label><br>
                                        <input required v-model="name" type="text" placeholder="Name" class="w3-input w3-border w3-margin-bottom"/>
                                    </div>
                                    <div>
                                        <label><b>Surrname</b></label><br>
                                        <input required v-model="surrname" type="text" placeholder="Surrname" class="w3-input w3-border w3-margin-bottom"/>
                                    </div>
                                    <div>
                                        <label><b>Password</b></label><br>
                                        <input required v-model="password" type="password" placeholder="Password" class="w3-input w3-border w3-margin-bottom"/>
                                    </div>
                                    <div>
                                        <label><b>Repeat password</b></label><br>
                                        <input required v-model="passwordRepeat" type="password" placeholder="Password" class="w3-input w3-border w3-margin-bottom"/>
                                    </div>
                                    <button v-if="!registrationSuccessful" type="submit" class="w3-button w3-block w3-green w3-section w3-padding">Register</button>
                                    <div v-if="registrationSuccessful" class="w3-panel w3-pale-green w3-border">
                                        <h3>Registration succesfull!</h3>
                                    </div>
                                </div>
                            </form>
                        </slot>
                    </section>
                    <footer class="modal-footer">
                        <slot name="footer">
                        <button type="button" class="btn-green" @click="close">
                            Close
                        </button>
                        </slot>
                    </footer>
                </div>
            </div>
        </div>
    </transition>
</template>

<style scoped>
h3 {
    margin: auto
}
.modal-backdrop {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: rgba(0, 0, 0, 0.3);
  display: flex;
  flex-wrap: nowrap;
  justify-content: center;
  align-items: center;
  overflow: auto;
  z-index: 1;
}

.modal {
  background: #ffffff;
  box-shadow: 2px 2px 20px 1px;
  overflow: auto;
  display: flex;
  flex-direction: column;
  border: double;
  border-color: #4aae9b;
  width: 100%;
}

.modal-header,
.modal-footer {
  padding: 5px;
  display: flex;
  flex-wrap: nowrap;
}

.modal-header {
  border-bottom: 1px solid #eeeeee;
  color: #ffffff;
}

.modal-footer {
  border-top: 1px solid #eeeeee;
  justify-content: flex-end;
  display: flex;
}

.modal-body {
  position: relative;
  padding: 20px 10px;
  display: flex;
  overflow: auto;
  width: 100%;
}

.btn-close {
  border: none;
  font-size: 20px;
  padding: 20px;
  cursor: pointer;
  font-weight: bold;
  color: #4aae9b;
  background: transparent;
}

.btn-green {
  color: white;
  background: #4aae9b;
  border: 1px solid #4aae9b;
  border-radius: 2px;
}

.modal-fade-enter,
.modal-fade-leave-active {
  opacity: 0;
}

.modal-fade-enter-active,
.modal-fade-leave-active {
  transition: opacity 0.5s ease;
}
</style>

<script>
import axios from 'axios';

export default {
  name: "Register",
  props: {
    show: {
      type: Boolean
    }
  },
  data() {
    return {
      username: "",
      email: "",
      name: "",
      surrname: "",
      password: "",
      passwordRepeat: "",
      registrationSuccessful: false
    };
  },
  methods: {
    close() {
      this.$emit("toggleRegister");
    },
    register: function() {
      const { username, email, name, surrname, password, passwordRepeat } = this;
      console.log("register")
      axios.post('http://localhost:5000/users', {
            username: username,
            email: email,
            name: name,
            surrname: surrname,
            password: password
      })
      .then(response => {
          this.registrationSuccessful = true
          this.username= ""
          this.email= ""
          this.name= ""
          this.surrname= ""
          this.password= ""
          this.passwordRepeat= ""
        })
      .catch(e => {
        this.errors.push(e)
      })
    }
  }
};
</script>