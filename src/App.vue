<script setup>
import { computed, ref } from 'vue'
import BasicInput from './components/BasicInput.vue';
// import md5 from 'md5';


// API
function callAPI() {
  return fetch('/api/users/token', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      login: login,
      email: email,
      password: password
      // password: md5(password) POUR SECURISER LE PASSWORD
    })
  })
    .then(response => response.json())
    .then(data => {
      console.log(data)
      return data
    })
    .catch(error => console.error(error))
}

// login
const login = ref('')
const errorMessageLogin = ref('')
const validMessageLogin = ref('')

function loginValid(event) {
  console.log("1")
  console.log(event)
  if (!isValidLogin(event.target.value)) {
    console.log("2")
    errorMessageLogin.value = 'Login is not valid'
  }
  else {
    errorMessageLogin.value = ''
  }
}

function isValidLogin(login) {
  return /^[a-zA-Z0-9]{3,}$/.test(login);
}

// email
const email = ref('')
const errorMessageEmail = ref('')
const validMessageEmail = ref('')

function emailValid(event) {
  console.log(event.target.value);
  if (!isValidEmail(event.target.value)) {
    errorMessageEmail.value = 'Email is not valid';
    console.log('Email is not valid');
  } else {
    email.value = event.target.value;
    errorMessageEmail.value = ''; // Efface le message si l'email est valide
    // messageEmail.style.color = ''; // Réinitialise la couleur du texte
    console.log('Email is valid');
  }
}

function isValidEmail(email) {
  return /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/.test(email);
}

// password
const password = ref('')
const errorMessagePassword = ref('')
const validMessagePassword = ref('')

function passwordValid(event) {
  console.log(event.target.value)
  if (!isValidPassword(event.target.value)) {
    errorMessagePassword.value = 'Password does not contain at least 8 characters, a capital letter, a lowercase letter and a number.'
    console.log('Password is not valid')
  }
  else {
    password.value = event.target.value
    errorMessagePassword.value = ''
    console.log('Password is valid')
  }
}

function isValidPassword(password) {
  // renvoit true si le mot de passe contient au moins 8 caractères, une majuscule, une minuscule et un chiffre
  return /^(?=.*\d)(?=.*[a-z])(?=.*[A-Z]).{8,}$/.test(password);
}

// password confirmation
const passwordConf = ref('')
const errorMessagePasswordConf = ref('')
const validMessagePasswordConf = ref('')

function passwordConfValid(event) {
  console.log(event.target.value)
  if (!isValidPasswordConf(event.target.value)) {
    errorMessagePasswordConf.value = 'Password confirmation does not match the password'
    console.log('Password confirmation does not match the password')
  }
  else {
    passwordConf.value = event.target.value
    errorMessagePasswordConf.value = ''
    console.log('Password confirmation match the password')
  }
}

function isValidPasswordConf(passwordConf) {
  // renvoit true si le mot de passe contient au moins 8 caractères, une majuscule, une minuscule et un chiffre
  return passwordConf === document.getElementById('password').value
}

// disabled
const disabled = computed(() => {
  return errorMessageLogin.value !== '' || errorMessageEmail.value !== '' || errorMessagePassword.value !== '' || errorMessagePasswordConf.value !== '' || login.value.length === 0 || email.value.length === 0 || password.value.length === 0 || passwordConf.value.length === 0 ? true : false
})

</script>

<template>


  <form class="styleClass" action="/login" method="post">
    <fieldset>
      <legend>Connexion</legend>

      <BasicInput id="Login" label="Login" type="text" placeholder="Laruiss" v-model="login"
        :errorMessage="errorMessageLogin" :validMessage="validMessageLogin" @blur="loginValid($event)"
        @update:modelValue="login = $event" />


      <BasicInput id="email" label="Email" type="email" placeholder="stanislas.ormieres@yahoo.com" v-model="email"
        :errorMessage="errorMessageEmail" :validMessage="validMessageEmail" @blur="emailValid($event)"
        @update:modelValue="email = $event" />

      <BasicInput id="password" label="Password" type="password" placeholder="Azerty.123" v-model="password"
        :errorMessage="errorMessagePassword" :validMessage="validMessagePassword" @blur="passwordValid($event)"
        @update:modelValue="password = $event" />

      <BasicInput id="confipassword" label="Confirmation password" type="password" placeholder="Azerty.123"
        v-model="passwordConf" :errorMessage="errorMessagePasswordConf" :validMessage="validMessagePasswordConf"
        @blur="passwordConfValid($event)" @update:modelValue="passwordConf = $event" />

      <!-- <div>
          <label for="Login">Login</label>
          <input type="text" id="Login" name="Login" placeholder="Enter the login" @blur="loginValid($event)">
          <p :class="messageClass">
            {{ messageLogin }}
          </p>
        </div>
        <div>
          <label for="email">Email</label>
          <input type="email" id="email" name="email" placeholder="Enter the email" @blur="emailValid($event)">
          <p>
            {{ messageEmail }}
          </p>
        </div>
        <div>
          <label for="password">Password</label>
          <input type="password" id="password" name="password" placeholder="Enter the password"
            @blur="passwordValid($event)">
          <p :class="messageClass">
            {{ messagePassword }}
          </p>
        </div>
        <div>
          <label for="confipassword">Confirmation password</label>
          <input type="password" id="confipassword" name="confipassword" placeholder="Enter the password"
            @blur="passwordConfValid($event)">
          <p :class="messageClass">
            {{ messagePasswordConf }}
          </p>
        </div> -->
      <!-- 
      <div>
        <label for="login">Login</label>
        <input name="login" type="login" id="login">
        <p class="message"></p>
      </div>
      <div>
        <label for="email">email</label>
        <input name="email" type="email" v-model="email" id="email" @focus="onFocus($event)" @blur="onBlur($event)">
        <p>
          {{ email }}
        </p>
        <p class="message"></p>
      </div>
      <div>
        <label for="password">Password</label>
        <input name="password" type="password" id="password">

        <p class="message"></p>
      </div>
      <div>
        <label for="password">Confirmation Password</label>
        <input name="password" type="password" id="password">
        <p class="message"></p>
      </div> -->
      <p>
        <button type="submit" :disabled="disabled" @click="callAPI()"><b>Se Connecter</b></button>
      </p>
    </fieldset>
  </form>
</template>

<style scoped>
  /* Styles pour le formulaire de connexion */

  /* Titre principal */
  h1 {
    text-align: center;
    font-size: 32px;
    margin-bottom: 40px;
    color: #333;
  }

  /* Conteneur du formulaire */
  form {
    max-width: 400px;
    margin: 0 auto;
    background-color: #f9f9f9;
    padding: 30px;
    border-radius: 10px;
    box-shadow: 0px 0px 20px rgba(0, 0, 0, 0.1);
  }

  /* Titre du formulaire */
  legend {
    font-size: 24px;
    font-weight: bold;
    color: #555;
    margin-bottom: 20px;
    text-align: center;
  }

  /* Labels */
  label {
    display: block;
    margin-bottom: 8px;
    color: #555;
  }

  /* Inputs */
  input[type="text"],
  input[type="email"],
  input[type="password"] {
    width: 100%;
    padding: 12px;
    margin-bottom: 20px;
    border: 1px solid #ddd;
    border-radius: 6px;
    box-sizing: border-box;
    font-size: 16px;
  }


  /* Messages d'erreur */
  .error-message {
    color: #dc3545;
    margin-top: 5px;
    font-size: 14px;
  }

  /* Messages de validation */
  .valid-message {
    color: #28a745;
    margin-top: 5px;
    font-size: 14px;
  }
  
  

  
</style>